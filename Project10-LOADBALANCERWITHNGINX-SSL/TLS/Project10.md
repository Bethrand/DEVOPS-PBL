# Project 10: LOAD BALANCER SOLUTION WITH NGINX & SSL/TLS  

**In this project you will register your website with LetsEnrcypt Certificate Authority, to automate certificate issuance you will use a shell client recommended by LetsEncrypt – cetrbot**

## TASK:

#### This project consists of two parts:

#### 1.    Configure Nginx as a Load Balancer
#### 2.    Register a new domain name and configure secured connection using SSL/TLS certificates


#### STEP 1   - *Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it NginxLB-P10 (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 - this port is used for secured HTTPS connections)*

#### 1. Connect and Install NginxLB on Ubuntu 20.04 EC2 instance

`ssh -i "PBDEV.pem" ubuntu@ec2-52-90-107-163.compute-1.amazonaws.com`

`sudo apt update`

`sudo apt install nginx -y`

`sudo systemctl status nginx`

![Nginx Status](./Images/Nginx%20Status.png)

#### 2   *Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses*

`sudo vi /etc/hosts`

```
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts

# Local DNS for Web Servers
172.31.83.67  web1
172.31.92.197 web2
```

#### 3  - *configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers, edit private IP of both web1 and web2, servernames, and #comment out  # include /etc/nginx/sites-enabled/*; *

`sudo vi /etc/nginx/nginx.conf`

```
}

http {

upstream myproject {
    server web1  weight=5;
    server web2  weight=5;
  }

server {
    listen 80;
    server_name www.domain.com;
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

![Nginx Status2](./Images/Nginx%20Status2.png)

#### your web1 and web2 must be up and running and your domain active and updated to type A records in AWS route 53 using your Elastic IP address of your NGINX LOAD BALANCER before you can open your domain name on your browser.

#### STEP 2   - *REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES*

#### 1. #`Search/lookup Elastic IP Address on AWS - Add new tag (key: Name, Value: NginxLB) - Allocate - Elastic IP Addresses - Actions - chk box - Actions - Associate Elastic IP Address - instance - choose an instance (NginxLB Instance) - Associate.`  # instruction 

#### 2. #`Search Route 53 on AWS to Set up a domain - open a new tab - Registered Domains - Choose a domain name - Hosted zones - Click on your Hosted zone name - create 1st record as bethrandosuji.click - paste Elastic IP of NginxLB - create records - Create a 2nd record add www to bethrandosuji.click - create records`  # instruction 

#### 3. Configure Nginx to recognize your new domain name. Update your nginx.conf with server_name www.<your-domain-name.com> instead of server_name www.domain.com

```
}

http {

upstream myproject {
    server web1  weight=5;
    server web2  weight=5;
  }

server {
    listen 80;
    server_name www.bethrandosuji.click;
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


#### 4. Check that your Web Servers can be reached from your browser using new domain name using HTTP protocol – http://<your-domain-name.com>

`http://bethrandosuji.click`

#### 5. Install certbot and request for an SSL/TLS certificate. Make sure snapd service is active and running.

`sudo snap install --classic certbot`

`sudo systemctl status snapd`

![snapd service active](./Images/snapd%20service%20active.png)

#### 6. Request your certificate (just follow the certbot instructions – you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file so make sure you have updated it)

`sudo ln -s /snap/bin/certbot /usr/bin/certbot`

`sudo certbot --nginx`

`youremail@gmail.com`

`yes`

`yes`

`enter`

#### 7. Test secured access to your Web Solution by trying to reach https://<your-domain-name.com

`https://www.bethrandosuji.click/`

![Secured Access](./Images/Secured%20Access.png)

#### 8. Set up periodical renewal of your SSL/TLS certificate, By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently. You can test renewal command in dry-run mode

`sudo certbot renew --dry-run`

#### 9. Best practice is to have a scheduled job that to run renew command periodically. Let us configure a cronjob to run the command twice a day. To do so, lets edit the crontab file with the following command:

`crontab -e`

`* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1`

`1`

`Ctrl + O`

`Enter`

`Ctrl + X`