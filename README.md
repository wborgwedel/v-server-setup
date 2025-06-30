# v-server-setup  
&nbsp;  
&nbsp;  


Table of contents  

[Setup ssh Connection](#setup-ssh-sonnection)  
- [generate ssh key](#generate-ssh-key)
- [transfer public key](#transfer-public-key)
- [Disallow ssh Password-login](#disallow-ssh-password\-login)  

[Setup Nginx](#setup-nginx)
- [install nginx](#install-nginx)  
- [reroute to aternative nginx landing page](#reroute-to-aternative-nginx-landing-page)  

&nbsp;  
&nbsp;  


## Setup ssh Connection
&nbsp;
### generate ssh key
&nbsp;  

Generating ssh-key by using:  

`ssh-keygen -t ed25519`  
&nbsp;  

### transfer public key
&nbsp;  

Transfer the public-ssh-key to server.  

`ssh-copy-id -i ~/.ssh/da_server_ed25519.pub wborgwedel@168.119.168.45`  
&nbsp;  


### Disallow ssh Password-login  
&nbsp;  

Configure the ssh service to reject password login via ssh.

in `/etc/ssh/sshd_config` edit configuration from `'#PasswordAuthentication yes'` to `'PasswordAuthentication no'`  
&nbsp;  

For loading config-changes the ssh-daemon have to be restarted.  

`systemctl restart ssh`  
&nbsp;  

## Setup Nginx
&nbsp;  

### install nginx
&nbsp;  

update the package  
`apt update`  
&nbsp;  

installing nginx via apt  
`apt install nginx -y`  
&nbsp;  

#### reroute to aternative nginx landing page  
&nbsp;  

create 'alternatives' directory  
`mkdir /var/www/alternatives`  
&nbsp;  

create alternatives.html  
`touch /var/www/alternatives/alternatives.html`  
&nbsp;  

add some HTML  
see content @ [project server](http://168.119.168.45:8081/)  
&nbsp;  

create config-file and add new route to alternative landing page


`server {`  
`  listen 8081;`  
`  listen [::]:8081;`  
`  root /var/www/alternatives;`  
`  index alternative.html;`  
`  location / {`  
`    try_files $uri $uri/ =404;`  
`  }`  
`}`  


restart nginx


test for functionality
