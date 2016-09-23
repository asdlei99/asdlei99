
ffmpeg h264  aac  编码

int encoderAAC(BOBOAACEncoder *encoder,uint8_t *inputBuffer,int inputSize,char *outputBuffer,int *outSize)
{
    /* AVFrame表示一祯原始的音频数据，因为编码的时候需要一个AVFrame,，
       在这里创建一个AVFrame,用来填充录音回调传过来的inputBuffer.里面是数据格式为PCM
       调用FFMPEG的编码函数 avcodec_encode_audio2（）后，将PCM-->AAC
       编码压缩为AAC格式，并保存在AVPacket中。
       再调用 av_interleaved_write_frame
       我们将它写入到指定的推流地址上,FFMPEG将发包集成在它内部实现里
     */
    //    pthread_mutex_lock(&encoder->mutex);
    AVFrame *frame = avcodec_alloc_frame();
    frame->nb_samples = encoder->pCodeCtx->frame_size;
    frame->format = encoder->pCodeCtx->sample_fmt;
    frame->sample_rate = encoder->pCodeCtx->sample_rate;
    frame->channels = encoder->pCodeCtx->channels;
    frame->pts = frame->nb_samples*frameIndex;
    av_init_packet(&encoder->packet);
    int gotFrame = 0;
    int ret = avcodec_fill_audio_frame(frame, encoder->pCodeCtx->channels, AV_SAMPLE_FMT_S16, (uint8_t*)inputBuffer, encoder->buffer_size, 0);
    if (ret<0) {
        printf("avcodec_fill_audio_frame error !\n");
        av_frame_free(&frame);
        av_free_packet(&encoder->packet);
        return -1;
    }
    encoder->packet.data = NULL;
    encoder->packet.size = 0;
    ret = avcodec_encode_audio2(encoder->pCodeCtx, &encoder->packet, frame, &gotFrame);
    frameIndex++;
    if (ret < 0) {
        printf("encoder error! \n");
        av_frame_free(&frame);
        av_free_packet(&encoder->packet);
        //        pthread_mutex_unlock(&encoder->mutex);
        return -1;
    }
    if (gotFrame == 1) {
        encoder->packet.stream_index = encoder->pStream->index;
        encoder->packet.pts = frame->pts;
        ret = av_interleaved_write_frame(encoder->pFormatCtx, &encoder->packet);
        printf("[AAC]: audio encoder %d frame success \n",frameIndex);
    }
    av_frame_free(&frame);
    av_free_packet(&encoder->packet);
    //    pthread_mutex_unlock(&encoder->mutex);
    return 1;
}


H264编码
编码过程与AAC一样，注意H264编码时，需要将源数据转换为YUV420P的格式，编码函数如下：

int encoderH264(BOBOAACEncoder *encoder,uint8_t *inputBuffer,size_t bufferSize)
{
    AVFrame *frame = avcodec_alloc_frame();
    frame->pts = picFrameIndex;
//    int ysize = encoder->pVideoCodecCtx->width *encoder->pVideoCodecCtx->height;
    av_init_packet(&encoder->videoPacket);
    int gotPicture = 0;
    //将原数据填充到AVFrame结构里，便于进行编码
    avpicture_fill((AVPicture *)frame, inputBuffer, encoder->pVideoCodecCtx->pix_fmt, encoder->pVideoCodecCtx->width, encoder->pVideoCodecCtx->height);
    encoder->videoPacket.data = NULL;
    encoder->videoPacket.size = 0;
    // 进行H264编码
    int ret = avcodec_encode_video2(encoder->pVideoCodecCtx, &encoder->videoPacket, frame, &gotPicture);
    picFrameIndex++;
    if (ret < 0) {
        printf("avcodec_fill_video_frame error !\n");
        av_frame_free(&frame);
        av_free_packet(&encoder->videoPacket);
        return -1;
    }
    // 成功编码一祯数据 
    if (gotPicture == 1) {
        encoder->videoPacket.stream_index = encoder->pVideoStrem->index;
        encoder->videoPacket.pts = frame->pts;
        //写入到指定的地方 由encoder->pFormatCtx 保存着输出的路径
        ret = av_interleaved_write_frame(encoder->pFormatCtx, &encoder->videoPacket);
    }
    av_frame_free(&frame);
    av_free_packet(&encoder->videoPacket);
    return 1;
}

RTMP推送
目前基于FFMPEG做为推流器，RTMP连接及推送音视频包都在其内部实现了，主要在函数

//指定输出格式为FLV，Path这里传个RTMP://XXXX/XXX
ret = avformat_alloc_output_context2(&ofmt_ctx, NULL, "flv", path);
// 这个函数进行了URL的映射 ,比如我们PATH 为RTMP协议的，FFMPEG内部将使用librtmp里面的握手连接等函数。
if (avio_open(&ofmt_ctx->pb, path,AVIO_FLAG_WRITE) < 0 ) {
    printf("failed to open output file \n");
    return NULL;
}


###参考： 
http://blog.csdn.net/win_lin/article/details/13776457
https://www.zybuluo.com/qvbicfhdx/note/126161

FFMPEG处理音频时间戳的主要逻辑
http://blog.csdn.net/win_lin/article/details/13512517