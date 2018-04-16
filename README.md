## WebSSH
A simple web application to be used as an ssh client to connect to your ssh servers. It is written in Python, base on Tornado and Paramiko.

### Preview
![Login](https://github.com/huashengdun/webssh/raw/master/preview/login.png)
![Terminal](https://github.com/huashengdun/webssh/raw/master/preview/terminal.png)

### Features
* SSH password authentication supported, including empty password.
* SSH public-key authentication supported, including DSA RSA ECDSA Ed25519 keys.
* Encrypted keys supported.
* Fullscreen terminal supported.
* Terminal window resizable.
* Compatible with Python 2.7-3.6.

### Instructions

#### Without Docker

```
git clone https://github.com/huashengdun/webssh.git
cd webssh
pip install -r requirements.txt
python main.py
```

#### With Docker
```
git clone https://github.com/huashengdun/webssh.git
cd webssh
docker build -t webssh:latest .
docker -p 8022:8022 --name WebSSH webssh:latest
```

### Options
```
# configure listen address and port
python main.py --address='0.0.0.0' --port=8000

# configure missing host key policy
python main.py --policy=reject

# configure logging level
python main.py --logging=debug

# log to file
python main.py --log-file-prefix=main.log

# more options
python main.py --help
```````

### Nginx config example for running this app behind an nginx server
```
location / { 
    proxy_pass http://127.0.0.1:8888;
    proxy_http_version 1.1;
    proxy_read_timeout 300;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Real-PORT $remote_port;
} 
```

### Tips
* Try to use Nginx as a front web server (see config example above) and enable SSL, this will prevent your ssh credentials from being uncovered. Also afterwards the communication between your browser and the web server will be encrypted as they use secured websockets.
* Try to use reject policy as the missing host key policy along with your verified known_hosts, this will prevent man-in-the-middle attacks. The idea is that it checks the system host keys file("~/.ssh/known_hosts") and the application host keys file("./known_hosts") in order, if the ssh server's hostname is not found or the key is not matched, the connection will be aborted.
