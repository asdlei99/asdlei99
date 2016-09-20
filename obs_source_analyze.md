2016年09月19日 星期一 16时09分07秒

>  1. obs-app.cpp  main 
>  2. obs-app.cpp  OBSApp::OBSInit()
> >     window-basic-main.cpp  void OBSBasic::OBSInit()
> >     1.  init log 
> >     2.  init config ---> get config
> >     3.  init module  ---> load module
> >     4.  init display (render, size, graphics,  hotkeys..... ... )
> >     
> >     
> 3.  resetaudio()
> 4.  resetvideo()
>      audio_thread
>      video_thread
>      obs_video_thread
> 5.  prepicture ---> encode ---> rtmp

 2016年09月19日 星期一 18时31分20秒
>  
>  

2016年09月20日 星期二 09时43分09秒
1.  obs 封装库进行开发：
>     需求-功能设计
