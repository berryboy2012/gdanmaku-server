gdanmaku-server
===============
![](https://img.shields.io/badge/license-GPLv3-blue.svg)
![](https://img.shields.io/badge/python-2.7-yellowgreen.svg)

Web-based danmaku server

## Installation

Install dependencies:

- python modules: `flask`, `gevent`, `pyredis`
- service: `redis`

Run `webserver.py` and open http://localhost:5000/ in your browser.

风蓝接锅人员请直接看下面（

### I love docker
You should have a vps first

Then you need to install the docker

```
 curl -fsSL get.docker.com -o get-docker.sh
 sudo sh get-docker.sh --mirror Aliyun
```

Start Docker
```
 sudo systemctl enable docker
 sudo systemctl start docker
```


Clone this project
```
git clone https://github.com/tuna/gdanmaku-server
cd gdanmaku-server
```

Get a redis docker and run

```
docker pull redis:alpine
docker run --name redis -v /var/lib/redis:/data -d redis:alpine
```

Modify `settings.py` or create a `settings_local.py` in the gdanmaku dir, and remember the `REDIS_HOST`(if you want to use it in Wechat, please Modify the Wechat_Token)
in your settings. Let's say, `myredis`.

Modify `Dockerfile`, you may want to change the `sources.list` part. Next we build the docker image of danmaku:

```
docker build --tag danmaku:dev .
```

We need to mount the code as volume to the docker container, and link redis to it. Try

```
docker run -it --rm --link redis:myredis -v /path/to/gdanmaku-server:/data/gdanmaku -p IP:Port:5000 danmaku:dev python2 gdanmaku/webserver.py
```
If failed please check the path (use pwd under gdanmaku-server to show the path), then change the path of the command.

Open your browser and visit <http://IP:port/>, you should see the danmaku web page.

If you wanna run danmaku service as a daemon, use

```
docker run -d --name danmaku --link redis:myredis -v /path/to/gdanmaku-server:/data/gdanmaku -p IP:Port:5000 danmaku:dev python2 gdanmaku/webserver.py
```
If you want to use it in Wecaht, please set the port to 80, and open the firewall.(鼻青脸肿.jpg
  
Good luck, and have fun!

## Client

The official desktop client is available at https://github.com/bigeagle/danmaQ
