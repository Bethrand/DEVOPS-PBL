# **Project 8: LOAD BALANCER SOLUTION WITH APACHE  - In this project we will enhance our Tooling Website solution by adding an Apache Load Balancer to distribute traffic between 2 Web Servers and allow users to access our website using a single LB URL**

*We will Implement this solution with 2 Web Servers srving as upstream web servers, the approach will be the same for 3 and more Web Servers*

## STEP 1 — *Deploy and configure an Apache Load Balancer for Tooling Website solution on a separate Ubuntu 20.04 EC2 instance and name it LBapache-Project. Make sure that users can be served by Web servers Web1-Apache and Web2-Nginx through the Load Balancer-Apache. We will save time by adding our security group for both Webservers and Apache-LB Server (TCP 80) at this point when spining the EC2 instances on all*

![Ec2Instance LBapache](./Images/EC2%20LBapache.png)

#### 2  - *Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:*

*Installing Web1-Apache (Upstream Webserver)*

`ssh -i "Project7.pem" ubuntu@ec2-44-203-142-78.compute-1.amazonaws.com`

`sudo apt update`

`sudo apt install apache2 -y`

`sudo systemctl status apache2`

![Apacher Webserver](./Images/Apache2%20Webserver.png)

*Installing Web2-NGINX (Upstream webserver)*

`ssh -i "Project7.pem" ubuntu@ec2-3-88-212-192.compute-1.amazonaws.com`

`sudo apt update`

`sudo apt install nginx -y`

`sudo systemctl status nginx`

![Nginx Webserver](./Images/Nginx%20Webserver.png)

*Installing Load Balancer-Apache2* 

`ssh -i "Project7.pem" ubuntu@ec2-52-91-214-154.compute-1.amazonaws.com`

`sudo apt update`

`sudo apt install apache2 -y`

`sudo apt-get install libxml2-dev`

`sudo a2enmod rewrite`

`sudo systemctl restart apache2`

`sudo a2enmod proxy`

`sudo systemctl restart apache2`

`sudo a2enmod proxy_balancer`

`sudo a2enmod proxy_http`

`sudo a2enmod headers`

`sudo a2enmod lbmethod_bytraffic`

*Restart apache2 service*

`sudo systemctl restart apache2`

#### 3 - *Configure load balancing*

`sudo vi /etc/apache2/sites-available/000-default.conf`

`<Proxy "balancer://mycluster">

            BalancerMember http://<WebServer1-Private-IP-Address>:80 loadfactor=5 timeout=1
            BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
            ProxySet lbmethod=bytraffic
          # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/`

`wqa!`

*Restart apache server*

`sudo systemctl restart apache2`

`sudo systemctl status apache2`

*Verify that our configuration works – try to access your LB’s public IP address or Public DNS name from your browser, Notice that whenever you refresh your browser, it alternates between Web1-Apache and Web2-Nginx webservers from the same LB access point or LB pub IP address:*

`http://52.91.214.154`

![LB Configuration](./Images/LB%20Apache%20Config1.png)

![Load Balancer Configuration](./Images/LB%20Apache%20Config2.png)

*Alternatively Configure Local DNS Names Resolution on LB hosts thus*

`sudo vi /etc/apache2/sites-available/000-default.conf`

`<Proxy "balancer://mycluster">
               BalancerMember http://web1:80 loadfactor=5 timeout=1
               BalancerMember http://web2:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>`

`wqa!`

*To accept our hosts file on the Load Balance Website:*

`sudo vi /etc/hosts`

`127.0.0.1 localhost
172.31.91.5 web1         - *edit private IP and assign host*
172.31.28.140 web2       - *edit private IP and assign host*

`wqa!`

`sudo systemctl restart apache2`

`sudo systemctl status apache2`

*Try to refresh your browser page http://Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php*

`http://52.91.214.154`

![LB Configuration](./Images/LB%20Apache%20Config1.png)

![Load Balancer Configuration](./Images/LB%20Apache%20Config2.png)

*The 2 webservers are now on a single LB access point.*
