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

