# MediaSource

## 为 Media Source Extensions 转码资产

在这之前需安装以下工具：

[ffmpeg](https://ffmpeg.org/)：一个开源的多媒体处理工具，它能够处理视频、音频以及其他多媒体文件和流的转换、编解码、剪辑、合并、流式传输等任务。它包含了一个广泛的库和工具集，支持几乎所有的音视频格式。FFmpeg 是视频编辑和流媒体处理的一个强大工具，广泛应用于媒体转换、流媒体服务、视频播放以及视频分析等领域。

### 碎片化

确保媒体文件是 MP4 文件，但没有正确分段，可以使用 ffmpeg：

```sh
ffmpeg -i non_fragmented.mp4 -movflags frag_keyframe+empty_moov fragmented.mp4
```

Chrome 都可能需要设置额外的电影标志：

```sh
-movflags frag_keyframe+empty_moov+default_base_moof
```

[More](https://developer.mozilla.org/en-US/docs/Web/API/Media_Source_Extensions_API/Transcoding_assets_for_MSE)
