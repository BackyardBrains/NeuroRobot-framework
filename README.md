# Neurorobot-Framework

Repository contains framework for NeuroRobot-iOS app and guidelines how to build framework's libraries.    

Framework is used to establish communication with RAK device.    

Used 3rd party libraries:  
- [boost](https://www.boost.org) (followed [this repo](https://github.com/faithfracture/Apple-Boost-BuildScript) to build it)  
- [ffmpeg](https://www.ffmpeg.org) (followed [this repo](https://github.com/kewlbear/FFmpeg-iOS-build-script) to build it)    

## Boost
Guidelines for building boost library

#### Windows


#### macOS | iOS

Used [this repo](https://github.com/faithfracture/Apple-Boost-BuildScript) to build `boost` library.    
Clone `Apple-Boost-BuildScript` from repo.  
Commands need to run in terminal from `Apple-Boost-BuildScript` folder.    
`./boost.sh -ios -macos --ios-archs "armv7 armv7s arm64 arm64e" --min-ios-version 10.3 --universal`  

## FFmpeg
Guidelines for building ffmpeg library

#### Windows

run `git clone https://github.com/FFmpeg/FFmpeg.git`  
get msys2 - https://www.msys2.org/  

launch msvc "developer command prompt".  
run from msys64 directory `msys2_shell.cmd -mingw64 -use-full-path`  

run in mingw:  
run `pacman -S make pkg-config diffutils`  
run `pacman -S mingw-w64-x86_64-nasm mingw-w64-x86_64-gcc mingw-w64-x86_64-SDL2`    
run `pacman -S yasm`  
maybe run `pacman -S make gcc diffutils mingw-w64-{i686,x86_64}-pkg-config mingw-w64-i686-nasm mingw-w64-i686-yasm`    
maybe run `export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig`    
maybe run `export PATH="path_to_lib.exe_in_msvc":$PATH`    
navigate to ffmpeg folder  
run `./configure --toolchain=msvc --arch=x86_64 --enable-shared --disable-static --prefix=./install`    
run `make`  
run `make install`    


from guide:  
https://www.linkedin.com/pulse/building-ffmpeg-windows-without-fuss-moshe-david  


run `./configure --disable-static --enable-shared --disable-doc --arch=x86_64 --target-os=win64 --prefix=/c/ffmpeg`  





#### macOS

run `git clone https://github.com/FFmpeg/FFmpeg.git`  
Edit the file `libavformat/rtpdec.c`. Line number 323: `rtcp_bytes /= 50;`, it should be: `rtcp_bytes /= 5;`. This is because of sending receive report more frequent.    
Commands need to run in terminal from ffmpeg folder.  
If you don't set `--prefix`, library will be in `/usr/lib`  
`./configure --enable-shared --prefix=./install`
if the error occurs, maybe you have to install yasm, command: `brew install yasm`  
`make`  
`make install`    

notes:  
- Libraries (*.dylib) must be in /usr/lib. So copy *.dylib files from FFmpeg-macOS/dylib to /usr/lib. (doesn't work, cannot copy, should rebuild whole library without `--prefix`)  
- Followed https://trac.ffmpeg.org/wiki/CompilationGuide/macOS  

#### iOS 

run `git clone https://github.com/kewlbear/FFmpeg-iOS-build-script.git`
(alternative: `https://github.com/tanersener/mobile-ffmpeg`)  
run `cd FFmpeg-iOS-build-script`  
edit value of `CONFIGURE_FLAGS` in `build-ffmpeg.sh` to be:  
```
--enable-cross-compile --disable-debug --disable-programs \
--disable-doc --enable-pic \
--enable-static --enable-shared --disable-avfilter --disable-avdevice --enable-swresample \
--enable-zlib --enable-bzlib --enable-iconv
```
and ARCHS to this:  
```
ARCHS="arm64 armv7 armv7s x86_64 i386"
```

run `./build-ffmpeg.sh`  
Stop the script after download and edit the file `libavformat/rtpdec.c`. Line number 323: `rtcp_bytes /= 50;`, it should be: `rtcp_bytes /= 5;`. This is because of sending receive report more frequent. After rerun `./build-ffmpeg.sh`.     
Libs will be in `FFmpeg-iOS` folder.  
