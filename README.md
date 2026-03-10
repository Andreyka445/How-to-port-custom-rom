# 🚀 Учимся портировать кастомные прошивки

> **Инструкция подходит для телефонов с разделом SUPER, внутри которого есть разделы system.img, system_ext.img, product.img**

## 📚 Содержание
- [Зачем это нужно](#зачем-это-нужно)
- [Инструменты](#🛠️-инструменты)
- [Выбор прошивки-донора](#🎯-выбор-прошивки-донора)
- [Важная теория](#🧠-важная-теория)
- [Способы портирования](#-способы-портирования)
  - [1️⃣ UKA (на телефоне)](#1️⃣-uka-на-телефоне)
  - [2️⃣ MIK (Windows)](#2️⃣-mik-windows)
  - [3️⃣ MIO Kitchen (Windows/Linux)](#3️⃣-mio-kitchen-windowslinux)
- [Прошивка готового порта](#📱-прошивка-готового-порта)
- [Решение проблем](#🆘-решение-проблем)

---

## Зачем это нужно

Не все телефоны имеют готовые кастомные прошивки. Этот метод позволяет взять прошивку от **другого устройства (донора)** и адаптировать её под **своё железо**, сохранив **родной vendor** (драйверы камеры, сенсора, связи и т.д.).

**Результат:** Вы получаете прошивку с интерфейсом и фишками донора (например, HyperOS), которая работает на вашем телефоне.

---

## 🛠️ Инструменты

| Инструмент | Назначение | Ссылка |
|------------|------------|--------|
| **UKA** | Портирование **на телефоне** (нужен Root) | [4PDA](https://4pda.to/forum/index.php?showtopic=900084) |
| **MIK** | Портирование на **Windows** (простой) | [4PDA](https://4pda.to/forum/index.php?showtopic=1017768) |
| **MIO Kitchen** | Портирование на **Windows/Linux** (самый мощный) | [GitHub](https://github.com/ColdWindScholar/MIO-KITCHEN-SOURCE) |
| **MT Manager** | Файловый менеджер на телефоне | [4PDA](https://4pda.to/forum/index.php?showtopic=548542) |
| **Platform-tools** | ADB и Fastboot | [Google](https://developer.android.com/tools/releases/platform-tools) |
| **Payload Dumper** | Распаковка payload.bin | [GitHub](https://github.com/vm03/payload_dumper) |

---

## 🎯 Выбор прошивки-донора

### 🔥 Золотые правила
1. **Чипсет:** Желательно **тот же производитель** (Unisoc → Unisoc, MediaTek → MediaTek, Qualcomm → Qualcomm).
2. **Android:** Желательно **та же версия** (Android 13 → Android 13). С разными версиями — высокий риск бутлупа.
3. **Оболочка:** Не имеет значения! Мы берём **только system**, а vendor оставляем свой.

---

## 🧠 Важная теория: что откуда берём

| Раздел | Откуда берём | Почему |
|--------|---------------|--------|
| **system** | **Донор** | Сама прошивка (интерфейс, приложения) |
| **system_ext** | **Донор** | Системные расширения (часть прошивки) |
| **product** | **Донор** | Продуктовый раздел (часть прошивки) |
| **vendor** | **Свой сток** | **ВАЖНО!** Драйверы железа (камера, сенсор, связь) |
| **odm** | **Свой сток** | Драйверы производителя (если есть) |
| **vendor_dlkm** | **Свой сток** | Модули ядра (если есть) |
| **boot** | **Свой сток** | Ядро (можно заменить на permissive при необходимости) |
| **vbmeta** | **Свой сток** | Для отключения верификации |

> **⚠️ КРИТИЧЕСКИ ВАЖНО:** В раздел **system_ext от ДОНОРА** нужно **обязательно** скопировать папку **APEX из своего СТОКОВОГО system_ext**. Иначе будут проблемы с совместимостью!

---

## 1️⃣ UKA (на телефоне)

### Подготовка
1. Установи **MT Manager** и **Termux**.
2. Скачай прошивку-донор (например, HyperOS).
3. Если прошивка в `payload.bin` — распакуй её через **MT Manager** (он умеет открывать payload) и извлеки `system.img`, `system_ext.img`, `product.img`.
4. Скопируй эти `.img` файлы в рабочую папку UKA:  
   **`/data/local/UnpackerSystem`**

### Запуск
Открой **Termux** и дай root-доступ:
```su -c menu```


Распаковка

В меню выбери:

    3 → 1 (Unpack system.img)

Повтори для system_ext.img и product.img.
Кухня покажет, в каком формате образы (EXT4 или EROFS).
Редактирование

    В папке /data/local/UnpackerSystem появились папки с распакованными образами.

    Открой их через MT Manager.

    ВАЖНО: В папку system_ext от донора скопируй папку apex из своего стокового system_ext (предварительно распакованного отдельно).

    Отредактируй build.prop в папках system, system_ext, product:

        Пропиши модель своего телефона (например, ro.product.model=Infinix X6525)

        Пропиши свой fingerprint (скопируй из стокового build.prop)

Упаковка

Вернись в Termux → menu → 7 → 2 (Pack in raw).
Выбери тип упаковки:

    Если образы были EROFS → пункт 6 (из EROFS в EROFS)

    Если EXT4 → пункт из EXT4 в EROFS (если твой сток использует EROFS)

У тебя появятся файлы с припиской _new.
Сборка super.img в UKA

    Распакуй стоковый super через UKA: menu → 3 → 2.

    Удали из распакованного super старые system.img, system_ext.img, product.img.

    Скопируй туда новые образы (с припиской _new).

    В UKA: menu → 7 → 3 → 1 (сборка в sparse).

    Новый super появится в папке _out.

2️⃣ MIK (Windows)
Установка

    Скачай MIK с GitHub.

    Распакуй в корень диска C:\MIK.

    Запусти MIK.exe.

Портируем

    Распакуй скачанную прошивку-донор.

    Вытащи из неё system.img, system_ext.img, product.img.

    Распакуй эти образы через MIK.

Редактирование

    В build.prop'ах пропиши своё устройство (модель, fingerprint).

    ОБЯЗАТЕЛЬНО в system_ext от донора скопируй папку APEX из стокового system_ext (его тоже распакуй через MIK).

    Распакуй стоковый super, удали из него system.img, system_ext.img, product.img.

    Скопируй новые образы в super.

    Если нужно, в стоковый vendor можно добавить Battery Honey.

    Собери новый super в sparse (для прошивки через fastboot) или в raw (для TWRP).

    ⚠️ ВАЖНО: MIK не умеет менять файловую систему. Он упакует в ту же ФС, что была у исходного образа.

3️⃣ MIO Kitchen (Windows/Linux)
📥 Установка

    Скачай последнюю версию MIO Kitchen

    Распакуй архив (желательно в корень диска C:\ или в папку без пробелов)

    Запусти:

        Windows: дважды кликни MIOKitchen.exe

        Linux: сделай .AppImage исполняемым и запусти

🏗️ Шаг 1: Создаём проекты

ВАЖНО: Нужно создать ДВА ОТДЕЛЬНЫХ ПРОЕКТА:

    Проект STOCK — для стоковой прошивки

    Проект PORT — для прошивки-донора

Создаём проект STOCK:

    Нажми "Create Project"

    Назови, например, Infinix_Stock

    Укажи папку для проекта

    При запросе super.img выбери super.img из стоковой прошивки

Создаём проект PORT:

    Снова нажми "Create Project"

    Назови, например, HyperOS_Port

    Укажи другую папку

    При запросе super.img выбери super.img из прошивки-донора

📤 Шаг 2: Распаковываем образы
В проекте PORT:

    Перейди в "Image Tools" → "Unpack"

    Распакуй образы донора:

        system.img → в папку project/system

        system_ext.img → в project/system_ext

        product.img → в project/product

В проекте STOCK:

    Распакуй только system_ext.img из стока:

        system_ext.img → в project/system_ext (нужно для APEX)

✏️ Шаг 3: Редактируем

    Замени APEX:

        Из проекта STOCK (папка system_ext/apex) скопируй папку apex

        Вставь в проект PORT (папка system_ext/), заменив существующую

    Отредактируй build.prop в проекте PORT:

        project/system/build.prop

        project/system_ext/build.prop

        project/product/build.prop

    Пропиши модель своего телефона и обязательно замени fingerprint на стоковый!

📥 Шаг 4: Упаковываем образы

В проекте PORT:

    Перейди в "Repack" → "Pack from folder"

    Упакуй каждую папку обратно в .img:

        Из system → system.img

        Из system_ext → system_ext.img

        Из product → product.img

Все новые образы появятся в папке _output.
🧩 Шаг 5: Собираем финальный super.img

    В проекте PORT перейди в "Super Image Tools" → "Repack Super"

    Укажи:

        Source super file: путь к super.img из проекта PORT (который ты указал при создании)

        New images folder: путь к папке _output (там лежат новые system.img и т.д.)

    Нажми "Repack"

Готовый super_new.img появится в папке _output.
📱 Прошивка готового порта
Через Bootloader (fastboot)

    Перезагрузи телефон в fastboot:
    bash

adb reboot bootloader

    ⚠️ ВАЖНО: Нужен именно fastboot, а не fastbootd!

Прошей новый super:
bash

fastboot flash super super_new.img

Сотри данные:
bash

fastboot -w

Перезагрузись:
bash

fastboot reboot

Если ошибка dm-verify corrupt — отключи верификацию:
bash

fastboot --disable-verity flash vbmeta vbmeta.img

    (повтори для vbmeta_system, vbmeta_vendor, если есть)

Через TWRP/OFRP

    Перезагрузи в рекавери:

    adb reboot recovery

    ОБЯЗАТЕЛЬНО сделай backup раздела super.

    Прошей super_new.img (в TWRP: Install → Install Image → выбери super.img → выбери раздел super).

    Сделай Format Data (не просто wipe, а именно format).

    Перезагрузись.

🆘 Решение проблем
Проблема	Решение
Бутлуп на лого	Прошей boot-permissive.img
Ошибка dm-verify	Прошей vbmeta с флагами --disable-verity --disable-verification
Телефон не видит SIM	Проблема с RIL — нужна замена libs в vendor
Не работает камера	Замени библиотеки камеры из стока в vendor
WiFi не включается	Замени модули WiFi из стока в vendor_dlkm
Гугл сервисы крашатся	Проверь fingerprint в build.prop
Не хватает места в super	Уменьши размер system (удали лишние приложения/обои)
🙏 Благодарности

    Andreyka445 — за основу инструкции

    ColdWindScholar — за MIO Kitchen

    CryptoNickSoft — за MIK

    Сообщество 4PDA — за бесценный опыт

Удачи в портировании! 🚀
