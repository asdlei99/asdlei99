2016年09月23日 星期五 15时39分51秒
###  linux test audio device
command:
> 1. List of sound devices
>      cat /proc/asound/cards
>      arecord -l
> 2. Test MIC
>      arecord -d 10 /tmp/test-mic.wav
> 3. Play test-mic.wav
>      aplay /tmp/test-mic.wav
>      arecord  hw:1,0 -d 10  /tmp/test-mic.wav

参考：https://linuxconfig.org/how-to-test-microphone-with-audio-linux-sound-architecture-alsa
使用：（程序代码）http://www.cnblogs.com/lifan3a/articles/5481993.html


### portaudio
#### 使用方法：
cp lib/.libs/libportaudio.a /YOUR/PROJECT/DIR
cp /usr/local/lib/libportaudio.a /YOUR/PROJECT/DIR
You may also need to copy portaudio.h, located in the include/ directory of PortAudio into your project. Note that you will usually need to link with the approriate libraries that you used, such as ALSA and JACK, as well as with librt and libpthread. For example:
#### 编译例子：
gcc main.c libportaudio.a -lrt -lm -lasound -ljack -pthread -o YOUR_BINARY

#### 安装 及编译
参考： [编译链接](http://portaudio.com/docs/v19-doxydocs/compile_linux.html)
1. 前提条件： 
    sudo apt-get install libasound-dev
2. [下载源码编译](http://portaudio.com/download.html)
3.   ./configure && make
4.   sudo make install
