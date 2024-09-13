# FFmpeg

[FFmpeg](https://www.ffmpeg.org/about.html) is the leading multimedia framework, able to decode, encode, transcode, mux, demux, stream, filter and play
pretty much anything that humans and machines have created. It supports the most obscure ancient formats up to the
cutting edge. No matter if they were designed by some standards committee, the community or a corporation. It is also
highly portable: FFmpeg compiles, runs, and passes our testing infrastructure FATE across Linux, Mac OS X, Microsoft
Windows, the BSDs, Solaris, etc. under a wide variety of build environments, machine architectures, and configurations.

- [How to use](https://shotstack.io/learn/how-to-use-ffmpeg/)
- [Install on Rocky Linux](https://markaicode.com/how-to-install-ffmpeg-on-rocky-linux-9-almalinux-9/)
- [Using FFmpeg with Node](https://www.npmjs.com/package/fluent-ffmpeg)
    - [Example](https://github.com/ilkkamtk/hybrid-upload-server/blob/main/src/utils/getVideoThumbnail.ts)


Download video to /var/www/html:
```shell
cd /var/www/html/
sudo wget https://download.blender.org/peach/bigbuckbunny_movies/big_buck_bunny_1080p_h264.mov
```