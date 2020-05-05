how to build the super mario 64 pc port

binary will be in `build/sm64.us.f3dex2e`

NOTE: in these commands when you see -j9 replace that with the number of
cores/threads to use when compiling (in my case 9)

# Before Building

* get the sm64 US rom in z64 format, rename to `baserom.us.z64` and put it
  in the sm64pc root folder
* you can also use the japanese rom (except rename to .jp instead of .us)
  and when you run make you would pass `VERSION=jp`

# Building on Linux

* install gcc make glfw-devel libusb-devel audiofile-devel sdl2-devel. on raspbian
  for example it's `sudo apt install libglfw3-dev libusb-1.0-0-dev libaudiofile-dev libsdl2-dev

```sh
make -C tools clean
make -j9 TARGET_N64=0 clean
make -j9 TARGET_N64=0
```

* if you get errors with the assembler, open
  `sound/sequences/00_sound_player.s` in a text editor and
  replace all occurrences of `#` with `//`. it happened when trying to
  build on raspberry pi, probably because the arm assembler doesn't
  understand `#` comments

# Building on Windows

* install msys2 and start "MSYS2 MinGW 64-bit"
* `pacman -Syu`
* restart msys2
* `explorer .`
* copy sm64pc source code folder to the directory that opens in explorer

```sh
pacman -Syu
pacman -S git make auto{make,conf} libtool python3 gcc \
  mingw-w64-x86_64-gcc mingw-w64-x86_64-SDL2 mingw-w64-x86_64-pkg-config

cd sm64pc

git clone https://github.com/mpruett/audiofile/ --depth=1
cd audiofile
CC=x86_64-pc-msys-gcc CXX=x86_64-pc-msys-g++ ./autogen.sh --prefix=/usr/
cd libaudiofile
make -j9
make install
cd ../..

make -C tools clean
make -C tools CC=x86_64-pc-msys-gcc CXX=x86_64-pc-msys-g++
make -j9 TARGET_N64=0
```
