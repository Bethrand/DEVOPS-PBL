# LOAD BALANCER WITH NGINX & SSL/TLS

![README P10](./Images/README%20P10.png)

1. Spin EC2 Instance on Ubuntu 20.04 NginxLB Open TCP port 80 HTTP and TCP port 443 HTTPS

2. Search/lookup Elastic IP Address on AWS - Add new tag (key: Name, Value: NginxLB) - Allocate - Elastic IP Addresses - Actions - chk box - Actions - Associate Elastic IP Address - instance - choose an instance (NginxLB Instance) - Associate.

Search Route 53 on AWS to Set up a domain - open a new tab - Registered Domains - Choose a domain name - Hosted zones - Click on your Hosted zone name - create 1st record as bethrandosuji.click - paste Elastic IP of NginxLB - create records - Create a 2nd record add www to bethrandosuji.click - create records

3. Now we have domain name and route 53 all connected together with an IP address.

4. You might have noticed, that every time you restart or stop/start your EC2 instance – you get a new public IP address. When you want to associate your domain name – it is better to have a static IP address that does not change after reboot. Elastic IP is the solution to this change.

5. Connect instance and run NGINX Installation and config.

6. Using the hosts file to map a domain to an IP address is inportant when you want to test your website without changing the domain DNS settings. For instance, you are migrating your website to a new server, and you want to verify whether it is fully functional before pointing the domain to the new server. The hosts file can also be used to block websites on your computer. The hosts file has priority over DNS. When you type in the domain name of a web site you want to visit, the domain name must be translated into its corresponding IP Address. The operating system first checks its hosts file for the corresponding domain, and if there is no entry for the domain, it will query the configured DNS servers to resolve the specified domain name. This affects only the computer on which the change is made, rather than how the domain is resolved worldwide.

7. The IP address and the domain names should be separated by at least one space or tab. The lines starting with # are comments and are ignored

8. Elastic IP Address - Add new tag (key: Name, Value: NginxLB) - Allocate - Elastic IP Addresses - Actions - chk box - Actions - Associate Elastic IP Address - instance - choose an instance (NginxLB Instance) - Associate - 