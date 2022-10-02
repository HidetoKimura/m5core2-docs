# M5Stack NES Emulator

M5 Stack（第一世代）でファミコン（NES）を動かしていたのでやってみた。\
結果からいうと、M5Stackでは動いたがM5 Core2では途中で止まってしまっていた。\
環境もかなり異なるため、調査が必要。\
\
参考ページ\
[https://macsbug.wordpress.com/2018/05](https://macsbug.wordpress.com/2018/05/07/nes-game-with-m5stack/)

Ubuntu 20.04だとpython2系が廃止されているのでバックポートする

```
$ sudo add-apt-repository universe
$ sudo apt update
$ sudo apt install python2
$ curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py
$ sudo python2 get-pip.py
$ pip2 --version
```

公開されているエミュレータはESP-IDF v3.3なのでそのバージョンを落としてくる

```
$ mkdir m5stack-nes
$ cd m5stack-nes/
$ git clone https://github.com/espressif/esp-idf.git -b release/v3.3
$ git clone https://github.com/m5stack/M5Stack-nesemu.git

$ cd esp-idf/
$ git submodule update --init --recursive
$ git log -n1
$ ./install.sh
$ . export.sh
$ cd ..
$ cp -r ~/work/m5core2/m5stack-nes/esp-idf/examples/get-started/hello_world/ .

$ ls
M5Stack-nesemu  esp-idf  hello_world
```

hello worldが動くかみる

```
$ cd hello_world/
$ make menuconfig
Serial Console Default  -> /dev/ttyACM0

$ make
$ make flash
$ make monitor
```

うごいたら、今度はnesをビルドする

```
$ cd M5Stack-nesemu/
$ make menuconfig
Serial Console Default  -> /dev/ttyACM0
$ make
$ make flash
$ make monitor
```

ビルドも書き込みもできたが絵が出ない。オリジナルを調べる必要あり。
