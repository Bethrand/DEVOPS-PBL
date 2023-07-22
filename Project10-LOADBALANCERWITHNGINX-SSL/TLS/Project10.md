# Project 8: LOAD BALANCER SOLUTION WITH NGINX & SSL/TLS  

**In this project you will register your website with LetsEnrcypt Certificate Authority, to automate certificate issuance you will use a shell client recommended by LetsEncrypt – cetrbot**

## TASK:

#### This project consists of two parts:

#### 1   - Configure Nginx as a Load Balancer
#### 2   - Register a new domain name and configure secured connection using SSL/TLS certificates


#### STEP 1   - *Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it NginxLB-P10 (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 - this port is used for secured HTTPS connections)*

`ssh -i "PBDEV.pem" ubuntu@ec2-52-90-107-163.compute-1.amazonaws.com`

`sudo apt update`

`sudo apt install nginx -y`

`sudo systemctl status nginx`

![Nginx Status](./Images/Nginx%20Status.png)

#### 2 -  *Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses*

`sudo vi /etc/hosts`

```
127.0.0.1 localhost
172.31.86.226 web1
172.31.86.108 web2

# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts 
```

#### 3  - *Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers, edit private IP of both web1 and web2, servernames, and # out  # include /etc/nginx/sites-enabled/*; *

`sudo vi /etc/nginx/nginx.conf`

```
    # Virtual Host Configs
        ##

upstream myproject {
    server web1 weight=5;
    server web2 weight=5;
  }

server {
    listen 80;
    server_name bethrandosuji.click www.bethrandosuji.click;
    location / {
      proxy_pass http://myproject;
    }
  }

          include /etc/nginx/conf.d/*.conf;
        # include /etc/nginx/sites-enabled/*;
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
```

`:wqa!`

`sudo nginx -t`

`sudo systemctl restart nginx`

`sudo systemctl status nginx`

``

#### 2  - *Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:*

