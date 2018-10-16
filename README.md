gdanmaku-server
===============
![](https://img.shields.io/badge/license-GPLv3-blue.svg)
![](https://img.shields.io/badge/python-3.7-yellowgreen.svg)

Web-based danmaku server

## Installation

### The easy way

0. Get yourself a VPS

1. Clone this project
    ```bash
    git clone https://github.com/tuna/gdanmaku-server
    cd gdanmaku-server
    ```
    
    Pay attention to where you see this guide and what repository you are cloning from.
    Actual repository address may differ.
    
2. Install `openssl` `curl` `python3` `nano` if there isn't

3. Install `docker` and `docker-compose` if there isn't
    ```bash
    sudo snap install docker
    sudo curl -L --fail https://github.com/docker/compose/releases/download/1.22.0/run.sh -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    ```
    
    > We recommend you use docker pre-installed images from your VPS provider 
    since there are many caveats when installing them yourself. 

4. Edit configs to your preference

    ```bash
    python3 -c "import random,hashlib;print(hashlib.sha1(str(random.random()).encode()).hexdigest())"
    ```
    
    Copy the output, we will call it \<SECRET\> in the following easy steps.
    
    ```bash
    curl -4 icanhazip.com
    ```
    This should be your public ip, we will use it in the following steps.
    
    If you want to use Wechat, log into your Wechat Subscription Account, get your `Token`.
    
    If you want to use telegram, get your telegram token.
    Now we can start editing configs
    ```bash
    cd docker_conf
    nano settings_local.py
    ```
    Copy the following contents into it.
    ```python
    DEBUG = False

    SECRET_KEY = "<SECRET>"

    # WECHAT
    WECHAT_TOKEN = "<WECHAT_TOKEN>"

    # TELEGRAM
    TELEGRAM_TOKEN = "<TELEGRAM_TOKEN>"
    ```
    Change \<SECRET\>, \<WECHAT_TOKEN\>, \<TELEGRAM_TOKEN\> according to your situation.
    
    After that you can press <kbd>Ctrl</kbd> + <kbd>O</kbd> to save and <kbd>Ctrl</kbd> + <kbd>X</kbd> to exit.
    
5. Run the script and open corresponding ports
    ```bash
    chmod +x ./easy_setup.sh
    sudo ./easy_setup.sh
    ```
    Wait until everything finished.
    
    > If you have trouble building docker image in executing the script above,
    you can try uncommenting the lines commented in PROJECT_ROOT_DIR/Dockerfile

    Then open port `80`, `443` in your VPS according to your OS and VPS provider.
    
    > If you do not use telegram support, then it is okay to keep port `443` closed. 

6. Edit Wechat Subscription Account if necessary

    Change `URL` in your account settings to 
    ```
    http://<PUBLIC_IP>/api/wechat
    ```
7. Finished

    Now you are all set, go to https://www.github.com/tuna/danmaQ and get a display client.
    
### The other way

Install dependencies:

- python modules: `flask`, `gevent`, `pyredis`
- service: `redis`

Run `webserver.py` and open http://localhost:5000/ in your browser.

> Use this method only if you want to see what's under the hood. 
To actually host this server, please use docker instead.

### Docker the hard way

You should have a vps first, then you need to install the docker if you do not have one.
> You can find help in [Prepare your Docker Environment](https://docs.docker.com/get-started/#prepare-your-docker-environment)
or your VPS provider's knowledge base.


Clone this project

Pay attention to where you see this guide and what repository you are cloning from.
    Actual repository address may differ.
```
git clone https://github.com/tuna/gdanmaku-server
cd gdanmaku-server
```

Get a redis docker and run

```
docker pull redis:alpine
docker run --name redis -v /var/lib/redis:/data -d redis:alpine
```

Modify `settings.py` in dir `gdanmaku`, remember `REDIS_HOST`in your settings i.e. `myredis`.

>If you want to use it in Wechat, please modify the `WECHAT_TOKEN` in `setting.py`.
Change `TELEGRAM_TOKEN` as well if you want to utilize telegram bot.

Modify `Dockerfile`, you may want to change the `sources.list` part. Next we build the docker image of danmaku:

```
docker build --tag danmaku:dev .
```

We need to link redis to it. Try:

```
docker run -it --rm --link redis:myredis -p IP:Port:5000 danmaku:dev python3 gdanmaku/webserver.py
```

Open your browser and visit <http://IP:port/>, you should see the danmaku web page.

If you wanna run danmaku service as a daemon, use

```
docker run -d --name danmaku --link redis:myredis -p IP:Port:5000 danmaku:dev python3 gdanmaku/webserver.py
```
If you want to use it in Wechat alone, please set `port` to 80, and open the firewall.

>If you want to use it in Telegram, however, you need to setup reverse proxy and a certificate besides set `port` to 5000.
>
>Here are the links for [Telegram Webhook Setup Tutorial](https://core.telegram.org/bots/webhooks) in your convenience.
>You may find `docker_conf/easy_setup.sh` and `docker_conf/nginx.conf` helpful.
  
Good luck, and have fun!

## Client
The official desktop client is available at https://github.com/tuna/danmaQ 

