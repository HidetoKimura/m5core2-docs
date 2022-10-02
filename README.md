# WSL2でのFlash書き込み環境

## やりたいこと

M5 Stack Core2をいじりたいが、Arduinoだとバックエンドが隠されて何をしているのかがわからないので、wsl2+vscodeで開発したい。ビルド等は問題ないが、書き込みをWindowsでやるのは非常に煩わしい。WSL2での書き込みをトライしてみる。

参考ページ

[https://qiita.com/baggio/items/28c13ed8ac09fc7ebdf1](https://qiita.com/baggio/items/28c13ed8ac09fc7ebdf1)\
[https://github.com/dorssel/usbipd-win/wiki/WSL-support](https://github.com/dorssel/usbipd-win/wiki/WSL-support)\
\


## 前提条件

```
> wsl --status
Kernel version: 5.10.60.1

> wsl --list
Windows Subsystem for Linux Distributions:
Ubuntu-20.04 (Default)
```

## Winドライバのインストール

書き込みはUSBシリアル経由で行う。WSL2で書き込みをできるようにするため、\
ホストとなるWindows側にドライバを入れておく。\


Arduinoのページ\
[https://docs.m5stack.com/en/quick\_start/core2/arduino](https://docs.m5stack.com/en/quick\_start/core2/arduino)

Espressifのページ\
[https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/establish-serial-connection.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/establish-serial-connection.html)\


## USBIPDをいれる

\
次にWindowsにUSBをGWするツール（USBIPD）を入れる。最新でよい。\
[https://github.com/dorssel/usbipd-win](https://github.com/dorssel/usbipd-win)\
\


M5Core2を接続してPower Shellで見てみる

```powershell
> usbipd wsl list 
BUSID VID:PID DEVICE STATE 
4-4 1a86:55d4 USB-Enhanced-SERIAL CH9102 (COM3) Not attached

```

WSL ubuntuに下記をインストール

```
$ sudo apt install linux-tools-virtual hwdata
$ sudo update-alternatives --install /usr/local/bin/usbip usbip /usr/lib/linux-tools/*/usbip 20
```

## デバイスのアタッチ・デタッチ

Power Shell側でアタッチする

```
> usbipd wsl attach --busid 4-4
```

Ubuntu側で検知しているか見る

```
$ lsusb
Bus 001 Device 004: ID 1a86:55d4 QinHeng Electronics USB Single Serial
```

デバイスに読み書き権限を与える。\
デバイスを差し込みなおすと忘れてしまうので再度設定。\
（グループで権限も与えてみたができなかった。要調査）

```shell
$ ls /dev/ttyACM*
/dev/ttyACM0

$ sudo chmod a+rw /dev/ttyACM0
$ ls -al /dev/ttyACM0
crw-rw-rw- 1 root root 166, 0 Sep 17 16:20 /dev/ttyACM0
```

Here is solution.\
[https://docs.platformio.org/en/latest/core/installation/udev-rules.html](https://docs.platformio.org/en/latest/core/installation/udev-rules.html)

```
# Recommended
curl -fsSL https://raw.githubusercontent.com/platformio/platformio-core/master/scripts/99-platformio-udev.rules | sudo tee /etc/udev/rules.d/99-platformio-udev.rules
```

```
PS C:\WINDOWS\system32> usbipd wsl list
BUSID  VID:PID    DEVICE                                                        STATE
2-2    10c4:ea60  Silicon Labs CP210x USB to UART Bridge (COM4)                 Not attached

Open /etc/udev/rules.d/99-platformio-udev.rules  then add "[60]" in {idProduct}.

# CP210X USB UART
ATTRS{idVendor}=="10c4", ATTRS{idProduct}=="ea[60][67][013]", MODE:="0666", ENV{ID_MM_DEVICE_IGNORE}="1", ENV{ID_MM_PORT_IGNORE}="1"
```

Restart udev service.

```
sudo service udev restart
```

Windows側につなぎたいときはデタッチする。

```
> usbipd wsl detach --busid 4-4
```
