# Учимся портировать кастомные прошивки
Иструкция подходит для телефонов с разделом _SUPER_, внутри которого есть разделы _system.img, system_ext.img, product.img_

# Содержание
[Зачем портировать прошивки?](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%D0%B7%D0%B0%D1%87%D0%B5%D0%BC-%D0%BF%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C-%D0%BF%D1%80%D0%BE%D1%88%D0%B8%D0%B2%D0%BA%D0%B8-%D0%B5%D1%81%D0%BB%D0%B8-%D0%B5%D1%81%D1%82%D1%8C-%D1%83%D0%B6%D0%B5-%D1%81%D0%BE%D0%B1%D1%80%D0%B0%D0%BD%D0%BD%D1%8B%D0%B5)

[Инструменты](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%EF%B8%8F-%D0%B8%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D1%8B-%D0%BA%D0%BE%D1%82%D0%BE%D1%80%D1%8B%D0%B9-%D0%BD%D0%B0%D0%BC-%D0%BD%D1%83%D0%B6%D0%BD%D1%8B)

[Выбор прошивки](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%D0%B2%D1%8B%D0%B1%D0%BE%D1%80-%D0%BF%D1%80%D0%BE%D1%88%D0%B8%D0%B2%D0%BA%D0%B8-%D0%BA%D0%BE%D1%82%D0%BE%D1%80%D1%83%D1%8E-%D0%B1%D1%83%D0%B4%D0%B5%D0%BC-%D0%BF%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D1%82%D1%8C)

[Портируем используя UKA](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%D0%BF%D0%BE%D1%80%D1%82%D0%B8%D1%80%D1%83%D0%B5%D0%BC-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D1%8F-uka--%D0%BF%D0%BE%D1%80%D1%82%D0%B8%D1%80%D1%83%D0%B5%D0%BC-%D0%BD%D0%B0-%D1%82%D0%B5%D0%BB%D0%B5%D1%84%D0%BE%D0%BD%D0%B5-)

[Портируем используя MIK](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%D0%BF%D0%BE%D1%80%D1%82%D0%B8%D1%80%D1%83%D0%B5%D0%BC-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D1%8F-mik-%D0%BD%D0%B0-%D0%BF%D0%BA-%D1%81-windows)

[Редактирование образов](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%D1%80%D0%B5%D0%B4%D0%B0%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D0%B1%D1%80%D0%B0%D0%B7%D0%BE%D0%B2)

[Сборка нового super.img](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%D1%81%D0%B1%D0%BE%D1%80%D0%BA%D0%B0-%D0%BD%D0%BE%D0%B2%D0%BE%D0%B3%D0%BE-superimg)

[Прошиваем новый super.img](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%D0%BF%D1%80%D0%BE%D1%88%D0%B8%D0%B2%D0%BA%D0%B0-superimg-%D1%81-%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%BC)

[Портируем с использованием MIK](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%D0%BF%D0%BE%D1%80%D1%82%D0%B8%D1%80%D1%83%D0%B5%D0%BC-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D1%8F-mik-%D0%BD%D0%B0-%D0%BF%D0%BA-%D1%81-windows)

[Портируем с использованием MIO](https://github.com/Andreyka445/How-to-port-custom-rom/tree/main#%D0%BF%D0%BE%D1%80%D1%82%D0%B8%D1%80%D1%83%D0%B5%D0%BC-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D1%8F-mio-%D0%BD%D0%B0-%D0%BF%D0%BA-%D1%81-linuxwindows)


# Зачем портировать прошивки, если есть уже собранные?
Ответ: Не все телефоны хорошо поддерживаются сообществом и не все имееют даже неоффициальную сборку TWRP, в которой работают все функции.
Портировать кастомную прошивку можно и без TWRP, просто собрав новый super.img в котором и будет наш порт, а если есть TWRP, который хоть что-то умеет прошивать то можно собрать прошиваемый .zip архив.

 # 🛠️ Инструменты которые нам нужны
 [UKA ( Unpacker Kitchen for Android )](https://4pda.to/forum/index.php?showtopic=900084)

 [MIK ( Multi Image Kitchen ) только для Windows](https://github.com/CryptoNickSoft/MIK)

 [MIO Kitchen Linux/Windows](https://github.com/ColdWindScholar/MIO-KITCHEN-SOURCE)
 
 [Termux](https://github.com/termux/termux-app/releases), если портируем на телефоне
 
 [Platform-tools ( Adb & Fastboot )](https://developer.android.com/tools/releases/platform-tools)

 
 Телефон на который будем портировать
 
 ___В моём случае это Tecno Pova 5 [LH7n/Rozen]___

 
[![pova-5.png](https://i.postimg.cc/K80bbpWZ/pova-5.png)](https://postimg.cc/18V2cBPT)

# Выбор прошивки, которую будем портировать
Для портирования прошивок A15 qpr1 и ниже подойдёт ваш стоковый vendor, но для того чтобы запусть А15 qpr2 нужен oss vendor, его можно взять из любой кастомной прошивки под ваш телефон.
Прошивку для портирования желательно брать с похожего на ваш телефон телефон, я портировал с Tecno Pova 4 на Tecno Pova 5, но также можно взять и прошивку с любого друго телефона, но желательно с тем же чипсетом и стоит у вас или хотябы портирвать с MTK на MTK также и с процессорам Qualcomm.


# Портируем используя UKA ( Портируем на телефоне )
Как файловый менеджер лучше всего использовать MT-manger, он работает как Toltal Commander в два окна, что очень удобно.
Я буду портирвать Evolution X 10.3 c Tecno Pova 4.
Скачиваем прошивку в моём случае она запакована в payload.bin, MT-manager позволяет открывать такие файлы, по этому я просто достаю из payload нужные для портирования 3 раздела это: __system.img, system_ext.img, product.img, далее перемещаем эти .img в рабочую папку UKA __/data/local/UnpackerSystem. Теперь открываем _Termux_ и даём ему ROOT доступ 
```
su
```
Далее 
```
menu
```
Мы попали в кухню.

[![photo-2025-08-30-13-32-12.jpg](https://i.postimg.cc/5y8Rz981/photo-2025-08-30-13-32-12.jpg)](https://postimg.cc/N2fDqcNP)

Сейчас нас интересует распаковка наших образов которые мы ранее положили в рабочую папку UKA.
Переходим по пунктам 3-1 и распаковываем образы при распаковке кухня покажет в каком формате собраны образы в EXT4 или EROFS.
После распаковки образов возврааемся в папку куда распаковались образы - /data/local/UnpackerSystem, если обрызы были в EROFS, то кухня создать одноименную папку, в которой буду находиться распакованные образы.

# Редактирование образов
Первым делом редактируем build.prop'ы прописываем своё устройсвто везде где упоминеться модель устройства, с которого портируем, если есть какието фиксы для исправления багов, то применяем их добавляем плюшки по типу Battarey Honey или добавляем нужные модули Magisk напрямую в систему.
 Теперь переходим к system_ext, прежде чем его запаковывать после редактирования, вы должны поместить в него папку APEX из стокового system_ext.img, а после можно запаковывать отредактированные образы. 
 В UKA переходим по пунктам: 7-2 ( сборка в raw , теперь внимательно смотри новые пункты нам нужно собрать новые образы в EROFS, если образы от из кастомной прошивки имели формат EROFS, то выбираем пункт 6 сборка из EROFS в EROFS, а если в EXT4, то выбираем сборку из EXT4 в EROFS. Выбор файловой системы зависит от вашего устройтсва у Pova 5 стоковые разделы по умолчанию идут EROFS, следовательно собираем новые разделы в EROFS. Если портируете с GAPPS, то собираете прошивку в EROFS  обязательно.

 # Сборка нового super.img
Удаляем старые образы и оставляем только образы с припиской _new.
Теперь нам нужна стоковая прошивка, а именно стоковый супер, который распаковываем через UKA по пунктам 3-2.
Удаляем из стокового _super_ system.img, system_ext.img, product.img, а на их место копируем новые образы, которые только что получили. Далее переходим в кухню и идёем по пунктам: 7-3-1 (сборка в sparse) собираем новый super.img он будет в папке _out_ и это всё порт готов.

# Прошивка super.img с портом 
___Прошивка через Bootloader___
1) подключаем телефон к пк, предварительно включив отладку по USB
2)  перезагружаемся в Fastboot __НЕ В FastbootD__, это важно, потому что через FastbootD прошиваются разделы, которые находяться внутри _super_
   ```
adb reboot bootloader
```
3) Находясь в режиме Fastboot прошиваем наш super.img, который скинули на пк в папку platfrorm-tools
   ```
   fastboot flash super super.img
   ```
 4) Далее форматируем дату
   ```
   Fastboot -w
   ```
 5) Перезагружаемся в систему
   ```
   fastboot reboot
   ```
   Если видим ошибку dm-verify corrupt, то отключаем вбмету (эта команда подходит для Pova 5 )
   ```
   fastboot --disable-verity flash vbmeta vbmeta.img
   ```
   ___Прошивка через TWRP/OFRP___
   
  1) Перезагружаемся в рекавери через Magisk или через adb
     
   ```
   adb reboot recovery
   ```
   1.1) __Делаем backup стокового super__

   2) Прошиваем super.img с портом
   3) Форматируем дату
   4) Перезагружаемся в систему

# Портируем используя MIK на пк с Windows

[![PXtibz0-Urq-Wny2-Rz0-CKgz1qm-Pt-J2s-J5z0-F4-Wa-DD.png](https://i.postimg.cc/zXQ8hgx5/PXtibz0-Urq-Wny2-Rz0-CKgz1qm-Pt-J2s-J5z0-F4-Wa-DD.png)](https://postimg.cc/yk0M4kqr)

 ___Устанавливаем MIK___
 1) Скачиваем
 2) Распаковываем ( желательно в корень диска C )
 3) Запускаем

___Портируем___

1) Распаковываем скачанную прошивку
2) Вытаскиваем из прошивки _system.img, system_ext, product.img___
3) Распаковываем через MIK

___Редактируем образы___
 1) В build.prop'ах прописываем своё устройство
 2) __ОБЯЗТЕЛЬНО__ в system_ext от портируемой прошивки копируем папку __APEX__ из стоквого system_ext ( его точно также распаковываем через MIK )
 3) После редактирования собираем образы в той файловой системе, в которой собраны образы стоковой прошивки
 4) Распаковываем стоковый супер удаляем из него system.img, system_ext.img, product.img
 5) Копируем новые образы в super
 6) Если нужно в стоковый vendor можно добавить Battarey Honey
 7) Собираем новый super в sparse, для прошивки через TWRP/OFRP можно собирать в raw

___Прошиваем порт___

___Прошивка через Bootloader___
1) подключаем телефон к пк, предварительно включив отладку по USB
2)  перезагружаемся в Fastboot __НЕ В FastbootD__, это важно, потому что через FastbootD прошиваются разделы, которые находяться внутри _super_
   ```
adb reboot bootloader
```
3) Находясь в режиме Fastboot прошиваем наш super.img, который скинули на пк в папку platfrorm-tools
   ```
   fastboot flash super super.img
   ```
 4) Далее форматируем дату
   ```
   Fastboot -w
   ```
 5) Перезагружаемся в систему
   ```
   fastboot reboot
   ```
   Если видим ошибку dm-verify corrupt, то отключаем вбмету (эта команда подходит для Pova 5 )
   ```
   fastboot --disable-verity flash vbmeta vbmeta.img
   ```
   ___Прошивка через TWRP/OFRP___
   
  1) Перезагружаемся в рекавери через Magisk или через adb
     
   ```
   adb reboot recovery
   ```
   1.1) __Делаем backup стокового super__

   2) Прошиваем super.img с портом
   3) Форматируем дату
   4) Перезагружаемся в систему

# Портируем используя MIO на пк с Linux/Windows



    


 
