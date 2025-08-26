# Учимся портировать кастомные прошивки
# Содержание

# Зачем портировать прошивки, если есть уже собранные?
Ответ: Не все телефоны хорошо поддерживаются сообществом и не все имееют даже неоффициальную сборку TWRP, в которой работают все функции.
Портировать кастомную прошивку можно и без TWRP, просто собрав новый super.img в котором и будет наш порт, а если есть TWRP, который хоть что-то умеет прошивать то можно собрать прошиваемый .zip архив.

 # 🛠️ Инструменты который нам нужны
 Андроид кухня, которая умеет распаковывать и запакоывать разделы и собирать super.img.
 
 [Termux](https://github.com/termux/termux-app/releases), если портируем на телефоне
 
 Инструменты adb&fastboot
 
 Телефон на который будем портировать
 
 ___В моём случае это Tecno Pova 5 [LH7n/Rozen]___

 
[![tecno-pova5-10.jpg](https://i.postimg.cc/CLLkxL0H/tecno-pova5-10.jpg)](https://postimg.cc/rz3KhTKK)

 # Кухни, которые работают на пк
 [MIO-kitchen](https://github.com/ColdWindScholar/MIO-KITCHEN-SOURCE)
 
 [MIK](https://github.com/CryptoNickSoft/MIK)
 
# Кухня, которая работает на телефоне

[UKA](https://4pda.to/forum/index.php?showtopic=900084) ___Только с ROOT правами___

# Platform-tools
[PT](https://developer.android.com/tools/releases/platform-tools)

Теперь, когда всё готово можем приступать к портированию!




 
