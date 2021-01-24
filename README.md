# deploy-muliple-domains-aws-apache-server
deploy-muliple-domains-in-same-apache-server


#### Launch AWS EC2 instance
- Login to aws (username and password)
- launch AWS EC2 insatance (generate .pem file)
- using generated .pem file login to server(example in ubuntu: `$ssh -i example.pem ubuntu@ip_address` (example: ip_address = 13.127.25.25)

#### Register Domain name in Route53
- Register Domain name from **AWS Route53** or using **GoDaddy** Domain in Hosted Zone by using namespace.
- create domain and subdomain in Hosted zones map map instance ip_address (example: create records like example.come, blog.example.com, profile.example.com map to 13.127.25.25).

#### install apache2 in lauched EC2 instance
- Install Apache Server
- `$ sudo apt install apache2`
- open nano editor or vi editor and map the domains blog, profile etc domains to ip address
- `$ nano /etc/hosts`

`127.0.0.1 localhost
13.127.25.25 example.com blog.example.com profile.example.com

::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts
`
- Create folders and add index.html code
 - `$ cd /var/www/html`
 - `$ mkdir blog`
 - `$ cd blog`
 - create index.html file
 - `$ nano index.html`
 `<html>
  <body>
    welcome to My Blog
  </body>
 </html>`
 
`$ mkdir profile`
`$ cd profile`
  - create index.html file
`$ nano index.html`
 `<html>
  <body>
    welcome to My Profile
  </body>
 </html>`
 
- Create apache conf file for domains and subdomains
`$ cd /etc/apache2/sites-available`
`$ nano blog.example.conf`

`<VirtualHost *:80>
        ServerAdmin webmaster@localhost
	      ServerName blog.example.com
	      ServerAlias blog.example.com
        DocumentRoot /var/www/html/blog

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>`

- nano profile.example.conf

`<VirtualHost *:80>
        ServerAdmin webmaster@localhost
	      ServerName profile.example.com
	      ServerAlias profile.example.com
        DocumentRoot /var/www/html/profile

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>`

- enable both confs and disable default conf file

`$ a2dissite 000default.conf
 $ a2ensite blog.example.conf
 $ a2ensite profile.example.conf`
 
 - Restart Apache server
`$ sudo service apache2 restart`
 
 #### Now Check in you browser
 - [blog.example.com](#) and [profile.example.com](#)



