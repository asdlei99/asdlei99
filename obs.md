
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())


// obs


http://jiantaofu.github.io/2015/07/12/obs%E8%A7%86%E9%A2%91%E9%87%87%E9%9B%86%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/

http://jiantaofu.github.io/2015/08/13/obs%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/


###obs_studio  ubuntu  安装
http://www.th7.cn/system/lin/201603/156148.shtml
Open Broadcaster Software
首先安装FFmpeg
添加源： 
sudo add-apt-repository ppa:kirillshkrogalev/ffmpeg-next 
更新源： 
sudo apt-get update 
安装： 
sudo apt-get install ffmpeg

如果是Ubuntu 15.04可以直接使用 
sudo apt-get install ffmpeg

安装obs-studio
添加源： 
sudo add-apt-repository ppa:obsproject/obs-studio 
更新源： 
sudo apt-get update 
安装： 
sudo apt-get install obs-studio



### obs_studio 编译安装
http://blog.csdn.net/csseikocs/article/details/51004602
https://github.com/jp9000/obs-studio/wiki/Install-Instructions#manually-compiling-on-redhat-based-distros-such-as-fedora
