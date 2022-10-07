# GDB Stub on ESP32

限定的な使い方だが、idf.py monitorではPANICが発生した場合にGDBを使うことができる。\
死ぬ間際の情報が取得できるのでデバッグ上、重宝する

(Top) → Component config → ESP System Settings → Panic handler behaviour\
Invoke GDB Stubを有効にする

<figure><img src=".gitbook/assets/config_gdbstub (1).png" alt=""><figcaption></figcaption></figure>

あとはプログラム中でassert(), abort()を呼び出せばよい。GDBが起動する。

```
abort() was called at PC 0x400d672f on core 0
0x400d672f: app_main at /home/kimura/work/m5core2/m5c2-base-v4.3.4/m5core2_esp-idf_template/build/../main/main.c:175 (discriminator 4)


Backtrace:0x40081663:0x3ffcb500 0x40087a91:0x3ffcb520 0x4008d4fa:0x3ffcb540 0x400d672f:0x3ffcb5b0 0x4010a5d0:0x3ffcb5f0 0x4008a55d:0x3ffcb610
0x40081663: panic_abort at /home/kimura/work/m5core2/m5c2-base-v4.3.4/esp-idf-v4.3.4/components/esp_system/panic.c:393

0x40087a91: esp_system_abort at /home/kimura/work/m5core2/m5c2-base-v4.3.4/esp-idf-v4.3.4/components/esp_system/system_api.c:112

0x4008d4fa: abort at /home/kimura/work/m5core2/m5c2-base-v4.3.4/esp-idf-v4.3.4/components/newlib/abort.c:46

0x400d672f: app_main at /home/kimura/work/m5core2/m5c2-base-v4.3.4/m5core2_esp-idf_template/build/../main/main.c:175 (discriminator 4)

0x4010a5d0: main_task at /home/kimura/work/m5core2/m5c2-base-v4.3.4/esp-idf-v4.3.4/components/freertos/port/port_common.c:145 (discriminator 2)

0x4008a55d: vPortTaskWrapper at /home/kimura/work/m5core2/m5c2-base-v4.3.4/esp-idf-v4.3.4/components/freertos/port/xtensa/port.c:168



ELF file SHA256: 803652585f4d147a

Entering gdb stub now.
$T0b#e6GNU gdb (crosstool-NG esp-2021r2-patch3) 9.2.90.20200913-git
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "--host=x86_64-build_pc-linux-gnu --target=xtensa-esp32-elf".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
--Type <RET> for more, q to quit, c to continue without paging--
Reading symbols from /home/kimura/work/m5core2/m5c2-base-v4.3.4/m5core2_esp-idf_template/build/basic.elf...
Remote debugging using /dev/ttyACM0
0x40081666 in panic_abort (details=0x3ffcb54b "abort() was called at PC 0x400d672f on core 0") at /home/kimura/work/m5core2/m5c2-base-v4.3.4/esp-idf-v4.3.4/components/esp_system/panic.c:404
404         *((int *) 0) = 0; // NOLINT(clang-analyzer-core.NullDereference) should be an invalid operation on targets
(gdb) 
```

\
\
[https://docs.espressif.com/projects/esp-idf/en/v4.3.4/esp32/api-guides/tools/idf-monitor.html](https://docs.espressif.com/projects/esp-idf/en/v4.3.4/esp32/api-guides/tools/idf-monitor.html)
