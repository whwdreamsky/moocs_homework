# Shadowsocks

> in ubuntu

```shell
sudo apt-get update
sudo apt-get install python
sudo apt-get install python-pip
pip install shadowsocks
sudo vim /etc/shadowsocks.json
ssserver -c /etc/shadowsocks.json -d start
# 开机自启
vi /etc/rc.local
```



### shadowsock json

~~~
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port：":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
~~~

