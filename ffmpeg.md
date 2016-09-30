PATH="$HOME/work/ffmpeg/bin:$PATH" PKG_CONFIG_PATH="/opt/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="$HOME/ffmpeg_build" \
  --pkg-config-flags="--shared" \
  --extra-cflags="-I/opt/ffmpeg_build/include" \
  --extra-ldflags="-L/opt/ffmpeg_build/lib" \
  --bindir="$HOME/work/ffmpeg/bin" \
  --enable-gpl \
  --enable-libass \
  --enable-libfdk-aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libtheora \
  --enable-libvorbis \
  --enable-libx264 \
  --enable-libx265 \
  --enable-nonfree
PATH="$HOME/work/ffmpeg/bin:$PATH" make


PATH="$HOME/work/ffmpeg/bin:$PATH" PKG_CONFIG_PATH="/opt/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="$HOME/ffmpeg_build" \
  --enable-shared \
  --extra-cflags="-I/opt/ffmpeg_build/include" \
  --extra-ldflags="-L/opt/ffmpeg_build/lib" \
  --bindir="$HOME/work/ffmpeg/bin" \
  --enable-gpl \
  --enable-libfdk-aac \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libtheora \
  --enable-libvorbis \
  --enable-libx264 \
  --enable-nonfree
PATH="$HOME/work/ffmpeg/bin:$PATH" make




ln -s /root/bin/ffmpeg /usr/bin/ffmpeg
ln -s /root/bin/ffplay /usr/bin/ffplay
ln -s /root/bin/ffprobe /usr/bin/ffprobe
ln -s /root/bin/ffserver /usr/bin/ffserver

=========ffmpeg complie in ubuntu 14.4.5========

http://www.liaoxuefeng.com/article/001456198314370db046cbe5e5a45388bf3ade4bc2c5cb0000

https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu

http://blog.csdn.net/u012891472/article/details/51482460

https://johnvansickle.com/ffmpeg/release-source/

=========test ffmpeg =============
https://ffmpeg.org/ffmpeg.html


========== 增加 ubuntu 空间方法；
http://blog.csdn.net/xuyuefei1988/article/details/8639788



===============vim ====
http://www.zhihu.com/question/27478597

https://github.com/hominlinx/vim

http://www.vim.org/

https://github.com/vim/vim

http://www.zhihu.com/question/23413774




================== vlc ==============

% sudo apt-get update
% sudo apt-get install vlc browser-plugin-vlc

http://www.videolan.org/vlc/download-sources.html





================================
-------yasm-1.3.0
./configure --prefix="/opt/ffmpeg_build" --bindir="/opt/bin"

------x264
PATH="/opt/bin:$PATH" ./configure --prefix="/opt/ffmpeg_build" --bindir="/opt/bin" --enable-static --disable-opencl

PATH="/opt/bin:$PATH" 
make

------x265
PATH="/opt/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="/opt/ffmpeg_build" -DENABLE_SHARED:bool=off ../../source


PATH="$HOME/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="$HOME/ffmpeg_build" -DENABLE_SHARED:bool=off ../../source


###x265
PATH="/opt/ffmpeg-2.8.8/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="/opt/ffmpeg-2.8.8/ffmpeg_build" -ENABLE_SHARED:bool=on ../../source
make
make install
make distclean


###fdk-aac
autoreconf -fiv
./configure --prefix="/opt/ffmpeg-2.8.8/ffmpeg_build" --enable-shared
make
make install
make distclean

### libvpx
PATH="/opt/ffmpeg-2.8.8/ffmpeg_bin:$PATH" ./configure --prefix="/opt/ffmpeg-2.8.8/ffmpeg_build" --enable-shared

PATH="/opt/ffmpeg-2.8.8/ffmpeg_bin:$PATH" make

### nvenc
cp nvEncodeApi.h /opt/ffmpeg-2.8.8/ffmpeg_build/include/
copy  nvEncodeApi.h  to /opt/ffmpeg-2.8.8/ffmpeg_build/include/

### ffmpeg 
PATH="/opt/ffmpeg-2.8.8/ffmpeg_bin:$PATH" PKG_CONFIG_PATH="/opt/ffmpeg-2.8.8/ffmpeg_build/lib/pkgconfig" ./configure \
  --prefix="/opt/ffmpeg-2.8.8/ffmpeg_build" \
  --extra-cflags="-I/opt/ffmpeg-2.8.8/ffmpeg_build/include" \
  --extra-ldflags="-L/opt/ffmpeg-2.8.8/ffmpeg_build/lib" \
  --bindir="/opt/ffmpeg-2.8.8/ffmpeg_bin" \
  --enable-shared \
  --enable-gpl \
  --enable-libass \
  --enable-libfdk-aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libtheora \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-nonfree \
  --enable-nvenc

  PATH="/opt/ffmpeg-2.8.8/ffmpeg_bin:$PATH" make