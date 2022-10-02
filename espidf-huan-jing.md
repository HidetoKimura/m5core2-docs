# ESP-IDF環境

## はじめに

M5 Core2はESP32というシステムチップが載っている。\
これにはメーカー（Espressif）からプログラミングガイドが提供されている。\
バージョンによって手順が異なるため、適宜バージョンで切り替えながらドキュメントを確認する必要がある。\
[https://docs.espressif.com/projects/esp-idf/en/latest/esp32/index.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/index.html)

## ESP-IDFでHello world

参考\
[https://zenn.dev/mai/articles/72519e289973c6](https://zenn.dev/mai/articles/72519e289973c6)\


手順がバージョンによってかなり異なるため、参考ページ通りにまずはやる。

```
$ pwd
~/work
$ git clone https://github.com/espressif/esp-idf.git -b v4.3.2
$ cd esp-idf
$ git submodule update
$ cd components/
$ git clone https://github.com/m5stack/M5GFX.git -b 0.0.15
$ cd M5GFX
$ git submodule update
$ cd ..
$ pwd
~/work/esp-idf
```

必要なツールチェーンをインストールし、環境変数を設定する

```
 $ ./install.sh
 $ . export.sh
```

ビルドをして書き込む

```
 $ cd examples/get-started/hello_world/
 $ idf.py build
 $ idf.py -p /dev/ttyACM0 flash
```

モニタをすると下記が出力される

```
$ idf.py -p /dev/ttyACM0 monitor
Executing action: monitor
Running idf_monitor in directory /home/kimura/work/m5core2/esp-idf/examples/get-started/hello_world
Executing "/home/kimura/.espressif/python_env/idf4.3_py3.8_env/bin/python /home/kimura/work/m5core2/esp-idf/tools/idf_monitor.py -p /dev/ttyACM0 -b 115200 --toolchain-prefix xtensa-esp32-elf- /home/kimura/work/m5core2/esp-idf/examples/get-started/hello_world/build/hello-world.elf -m '/home/kimura/.espressif/python_env/idf4.3_py3.8_env/bin/python' '/home/kimura/work/m5core2/esp-idf/tools/idf.py' '-p' '/dev/ttyACM0'"...
/home/kimura/work/m5core2/esp-idf/tools/idf_monitor.py:518: DeprecationWarning: distutils Version classes are deprecated. Use packaging.version instead.
  if StrictVersion(serial.VERSION) < StrictVersion('3.3.0'):
--- idf_monitor on /dev/ttyACM0 115200 ---
--- Quit: Ctrl+] | Menu: Ctrl+T | Help: Ctrl+T followed by Ctrl+H ---
ets Jul 29 2019 12:21:46

rst:0x1 (POWERON_RESET),boot:0x17 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:2
load:0x3fff0030,len:7412
load:0x40078000,len:14912
ho 0 tail 12 room 4
load:0x40080400,len:3688
0x40080400: _init at ??:?

entry 0x4008067c
I (29) boot: ESP-IDF v4.3.2 2nd stage bootloader
I (29) boot: compile time 11:38:47
I (29) boot: chip revision: 3
I (32) boot_comm: chip revision: 3, min. bootloader chip revision: 0
I (39) boot.esp32: SPI Speed      : 40MHz
I (44) boot.esp32: SPI Mode       : DIO
I (48) boot.esp32: SPI Flash Size : 2MB
I (53) boot: Enabling RNG early entropy source...
I (58) boot: Partition Table:
I (62) boot: ## Label            Usage          Type ST Offset   Length
I (69) boot:  0 nvs              WiFi data        01 02 00009000 00006000
I (76) boot:  1 phy_init         RF data          01 01 0000f000 00001000
I (84) boot:  2 factory          factory app      00 00 00010000 00100000
I (91) boot: End of partition table
I (96) boot_comm: chip revision: 3, min. application chip revision: 0
I (103) esp_image: segment 0: paddr=00010020 vaddr=3f400020 size=06ea8h ( 28328) map
I (122) esp_image: segment 1: paddr=00016ed0 vaddr=3ffb0000 size=02950h ( 10576) load
I (126) esp_image: segment 2: paddr=00019828 vaddr=40080000 size=067f0h ( 26608) load
I (139) esp_image: segment 3: paddr=00020020 vaddr=400d0020 size=13a74h ( 80500) map
I (169) esp_image: segment 4: paddr=00033a9c vaddr=400867f0 size=04adch ( 19164) load
I (177) esp_image: segment 5: paddr=00038580 vaddr=50000000 size=00010h (    16) load
I (183) boot: Loaded app from partition at offset 0x10000
I (183) boot: Disabling RNG early entropy source...
I (197) cpu_start: Pro cpu up.
I (197) cpu_start: Starting app cpu, entry point is 0x40081044
0x40081044: call_start_cpu1 at /home/kimura/work/m5core2/esp-idf/components/esp_system/port/cpu_start.c:150

I (0) cpu_start: App cpu up.
I (211) cpu_start: Pro cpu start user code
I (211) cpu_start: cpu freq: 160000000
I (212) cpu_start: Application information:
I (216) cpu_start: Project name:     hello-world
I (221) cpu_start: App version:      v4.3.2
I (226) cpu_start: Compile time:     Sep 17 2022 11:38:44
I (232) cpu_start: ELF file SHA256:  32811c7bcf2a5b3f...
I (238) cpu_start: ESP-IDF:          v4.3.2
I (243) heap_init: Initializing. RAM available for dynamic allocation:
I (250) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM
I (256) heap_init: At 3FFB3230 len 0002CDD0 (179 KiB): DRAM
I (263) heap_init: At 3FFE0440 len 00003AE0 (14 KiB): D/IRAM
I (269) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM
I (275) heap_init: At 4008B2CC len 00014D34 (83 KiB): IRAM
I (283) spi_flash: detected chip: generic
I (286) spi_flash: flash io: dio
W (290) spi_flash: Detected size(16384k) larger than the size in the binary image header(2048k). Using the size in the binary image header.
I (304) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
Hello world!
This is esp32 chip with 2 CPU core(s), WiFi/BT/BLE, silicon revision 3, 2MB external flash
Minimum free heap size: 291280 bytes
Restarting in 10 seconds...
Restarting in 9 seconds...
Restarting in 8 seconds...
Restarting in 7 seconds...
Restarting in 6 seconds...
Restarting in 5 seconds...
```

GFXのサンプルも動かしてみたが割愛する。ソースコードは下記。

[https://github.com/HidetoKimura/m5gfx\_study/tree/main/hello\_world](https://github.com/HidetoKimura/m5gfx\_study/tree/main/hello\_world)

