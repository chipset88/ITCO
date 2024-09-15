# Вводные данные

[К содержанию раздела](README.md)

## Железо

Разворачивать систему я буду на ноутбуке Acer Aspire 5755G. Его аппаратное обеспечение выглядит так:

|                     Параметр | Характеристики                                                      | Примечания                                                         |
| ---------------------------: | :------------------------------------------------------------------ | :----------------------------------------------------------------- |
|        **Материнская плата** | JV51_HR                                                             |
|                         BIOS | V1.20 1MiB 2560KiB                                                  | 03/16/2012                                                         |
|                       Память | SO-DIMM DDR3 1333 MHz 16GiB                                         | 2 планки по 8GiB                                                   |
|                    Процессор | Intel(R) Core(TM) i3-2310M CPU @ 2.10GHz                            |
|               VGA контроллер | 2nd Generation Core Processor Family Integrated Graphics Controller | `/dev/fb0`                                                         |
|                       Чипсет | **\*** MEI Controller #1                                            |
|           **USB контроллер** | **\*** USB Enhanced Host Controller #2                              |
|        USB хост (встроенный) | EHCI Host Controller                                                | _Linux 5.19.0-35-generic ehci_hcd_,<br> usb-2.00 speed = 480Mbit/s |
|                   Веб-камера | 1.3M HD WebCam: 1.3M HD WebCam                                      |
|         **Аудио-устройство** | **\*** High Definition Audio Controller                             |
|                     Микрофон | HDA Intel PCH Mic                                                   |
|                     Наушники | HDA Intel PCH Headphone                                             |
|                   HDMI-канал | HDA Intel PCH HDMI/DP,pcm=3                                         |
|                 **PCI-шина** | **\*** PCI Express Root (Port 1,2,4)                                |
|                Сетевая карта | NetLink BCM57785 Gigabit Ethernet PCIe                              |
|            Мультимедиа (MMC) | BCM57765/57785 SDXC/MMC Card Reader                                 |
|                Wi-Fi адаптер | AR9287 Wireless Network Adapter                                     | PCI-Express                                                        |
|           **USB контроллер** | uPD720200 USB 3.0 Host Controller                                   |                                                                    |
|                              | xHCI Host Controller                                                | _Linux 5.19.0-35-generic ehci_hcd_,<br>Гнездо USB 3.0              |
|            USB хаб (внешний) | USB2.0 HUB                                                          | За корпусом                                                        |
|                   Клавиатура | SEM USB Keyboard System Control                                     | Huasheng Electronics,За корпусом                                   |
|                         Мышь | A4Tech USB Mouse                                                    | За корпусом                                                        |
|                              | xHCI Host Controller                                                | _Linux 5.19.0-35-generic ehci_hcd_,<br>Гнездо USB 2.0              |
|           **USB контроллер** | **\*** USB Enhanced Host Controller #1                              |                                                                    |
|                              | xHCI Host Controller                                                | _Linux 5.19.0-35-generic ehci_hcd_,<br>Гнездо USB 2.0              |
|         USB хаб (встроенный) | USB-Integrated Rate Matching Hub                                    |                                                                    |
|            Bluetooth адаптер | Bluetooth Radio (Realtek)                                           |                                                                    |
|          **SATA контроллер** | **\*** 6 port Mobile SATA AHCI Controller                           |                                                                    |
|                     ATA Disk | ADATA SU650, 447GiB (480GB)                                         | SSD                                                                |
|                     ATA Disk | WDC WD3200BPVT-2, 298GiB (320GB)                                    | HDD                                                                |
|                    **SMBus** | **\*** SMBus Controller                                             | I²C                                                                |
|           Устройство питания | OEM_Define 75mWh                                                    |                                                                    |
|                      Батарея | CRB Battery 0                                                       |                                                                    |
|                 Power Button |                                                                     | `/dev/input/event0`                                                |
|                   Lid Switch |                                                                     | `/dev/input/event1`                                                |
|             Acer WMI hotkeys |                                                                     | `/dev/input/event11`                                               |
|                   Видео шина |                                                                     | `/dev/input/event13`                                               |
|                 Кнопка "Сон" |                                                                     | `/dev/input/event2`                                                |
|                 Power Button |                                                                     | `/dev/input/event3`                                                |
| AT Translated Set 2 keyboard |                                                                     | `/dev/input/event4`                                                |
|                      Тач-пад | ETPS/2 Elantech Touchpad                                            | `/dev/input/event10`                                               |

\* **_6 Series/C200 Series Chipset Family_**

## Назначение

Эта машина будет использоваться как терминал для администрирования сети, в которой будет располагаться, а также для разработки прикладных скриптов, простеньких программок и сайтов. Плюс к тому, я на ней буду вести конспекты, производить коммуникацию и смотреть видео. Также на ней считаю целесообразным тестировать программы, которые позволяют достигать задач, описанных выше. К таким относятся различные офисные программы и пакеты, системы планирования, текстовые редакторы и тому подобное. При этом на данной машине не бопустимо разворачивать сервера, если перечисленные программы являются клиент-серверными.

На данной машине не будет гипервизоров, веб-сервера, СУБД. На этой машине нельзя производить сомнительные операции и эксперименты, связанные с вмешательством в работу операционной системы. Для этого есть сервер виртуализации и машины, которые он поднимает. Крайне желательно код, который использует файловую систему и/или производит манипуляции с памятью (это планируется освавать позже) также запускать на виртуальных машинах.

Хранение данных должно быть лишь в том объёме, который я собираюсь проработать в ближайшую неделю, максимум две. Что требует наличия файлового хранилища в локальной сети.

## Причины выбора Xubuntu

Во-первых, я хочу работать на Linux, во-вторых, мне бы хотелось работать в Debian-подобном дистрибутиве. Так как моя машина - это ноутбук, то и графическая сиистема должна быть достаточно легковесна. XFCE подхоит под это требование. К тому же Xubuntu обладает достаточно неплохой документацией и по моему прежнему опыту стабильно работает.
