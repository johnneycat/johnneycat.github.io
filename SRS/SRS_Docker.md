# SRS Docker学习



srs github

https://github.com/ossrs/dev-docker



## 安装docker

https://www.docker.com/products/docker-desktop/



## 下载及编译

拉取代码

```shell
git clone https://gitee.com/ossrs/srs.git srs && cd srs/trunk && 
git remote set-url origin https://github.com/ossrs/srs.git && git pull
```

**Build SRS in dev docker**

```
cd ~/git/srs/trunk &&
docker run -it --rm -v `pwd`:/srs -w /srs ossrs/srs:dev \
    bash -c "./configure && make"
```

For common use

```
cd ~/git/srs/trunk &&
docker run -p 1935:1935 -p 1985:1985 -p 8080:8080 -p 8085:8085 \
     -it --rm -v `pwd`:/srs -w /srs ossrs/srs:dev \
    ./objs/srs -c conf/console.conf
```

For Webrtc

```
docker run -p 1935:1935 -p 1985:1985 -p 8080:8080 -p 8085:8085 \
    --env CANDIDATE=$(ifconfig en0 inet| grep 'inet '|awk '{print $2}') -p 8000:8000/udp \
     -it --rm -v `pwd`:/srs -w /srs ossrs/srs:dev \
    ./objs/srs -c conf/console.conf
```

对于webrtc来说，用户必须指明CANDIDATE：

https://github.com/ossrs/srs/wiki/v4_CN_WebRTC#config-candidate

* 使用rtmp推流，webrtc拉流：

  需要使用如下配置：

  ```shell
  docker run -p 1935:1935 -p 1985:1985 -p 8080:8080 -p 8085:8085 \
      --env CANDIDATE=$(ifconfig en0 inet| grep 'inet '|awk '{print $2}') -p 8000:8000/udp \
       -it --rm -v `pwd`:/srs -w /srs ossrs/srs:dev \
      ./objs/srs -c conf/rtmp2rtc.conf
  ```



SRS运行起来后，验证页面：

 http://localhost:8080/

推rtmp流：

```
ffmpeg -re -i ./doc/source.flv -c copy -f flv -y rtmp://localhost/live/livestream
```

拉流

* RTMP (by VLC):     rtmp://localhost/live/livestream

* H5(HTTP-FLV)：   [http://localhost:8080/live/livestream.flv](http://localhost:8080/players/srs_player.html?autostart=true&stream=livestream.flv&port=8080&schema=http)
* H5(HLS)：            [http://localhost:8080/live/livestream.m3u8](http://localhost:8080/players/srs_player.html?autostart=true&stream=livestream.m3u8&port=8080&schema=http)

* H5(WebRTC)        [webrtc://localhost/live/livestream](http://localhost:8080/players/rtc_player.html?autostart=true)



详细说明：

https://github.com/ossrs/srs

中文版 
https://github.com/ossrs/srs/wiki/v4_CN_Home#getting-started



android推流

https://blog.csdn.net/xiangang12202/article/details/122994219