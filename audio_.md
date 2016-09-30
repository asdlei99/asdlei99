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


###audacity 
终端运行 ： 即可打开 audacity
LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/ audacity

https://sukritkalra94.wordpress.com/2014/05/24/audacity-fails-with-the-pa_getstreamhostapitype-error/
Audacity Fails with the Pa_GetStreamHostApiType Error.

EDIT – Look for a better way to do this at the end of the post.

I was just running some PortAudio programs, when I saw that the maximum and the average amplitude reported by the recording were both 0.00. This meant that my microphone was not working and I immediately tried to start Audacity to test whether that was the case, since it gives a beautiful wave form for the recording in real time.

Running Audacity using GUI on Ubuntu 12.04 LTS, it failed to start and I tried restarting it a couple of times. Then, I went to the terminal and tried to start Audacity from there, at which point, it failed with the error:

audacity: symbol lookup error: audacity: undefined symbol: Pa_GetStreamHostApiType
Seeing the undefined symbol, Pa_GetStreamHostApiType, I knew that my PortAudio installation had messed something up.

So, to see the shared library dependencies of Audacity, I did:

ldd /usr/bin/audacity | grep portaudio
and sure enough, the dependency was pointing to /usr/local/lib.

So, the solution was to remove the libportaudio files from the /usr/local/lib, by doing:

rm -rf /usr/local/lib/libportaudio*
ldconfig
And, then I tried running Audacity and it finally started!

Another way to get your Audacity installation up and running without deleting shared libraries would be to give Audacity an extra directory to look for the libportaudio.so.* files. This is done using the LD_LIBRARY_PATH variable. Here’s a blog post explaining why LD_LIBRARY_PATH is bad.

Start Audacity using the following command then:

LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu/ audacity
If anyone cares, the reason why I wasn’t getting any input through the microphone was that somehow the Microphone had gotten muted inside the Sound Settings of Ubuntu!

I had to set it back to Unamplified and the microphones were back to life again!