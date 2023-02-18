# AlineHammermuller / Karl Cue

# How to set up NGINX and apply some basic configuation trough Digital Ocean
 
## Digital Ocean

1. Create an accout at Digital Ocean (https://www.digitalocean.com/)
	Could be the most basic account.
2. Open Windows terminal to generate and ssh key by typing ssh-keygen
3. Enter file in which to save the key (for exemple /Users/USER/.ssh/id_rsa)
	Change the name of the key (id_rsa) for another one related to the project
4. Enter passphrase (empty for no passphrase). Repeat the passphrase.
	If you wish you could just leave it empty.
5. Copy the public key using the command: cat ~/.ssh/<key.name>.pub
	Where <key_name> is the name given for your key in step 3.
6. Login at Digital Ocean, go to Settings (left menu) - Scurity - Add SSH Key
7. Paste de key and give it a meningful name.
8. Create a new project if do not have one or a new droplet if you already have a project
9. Select to use the primary key that you already created in your new droplet.

## Instaling NGIX

Login at your droplet using your ssh key, root user and IPV4 of your droplet.

> Example ssh -i ~\.ssh\key_name root@192.168.0.0

Check for apt updates

> sudo apt update

Install NGINX

> sudo apt install nginx

To check if the service is enable you can type

> systemctl status nginx.service

### Creating the HTML files and directories

The NGINX default content is stores at /var/www/html

Create a new directory by typing the following command and changing the IPV4 address
192.168.0.0 for your droplet IPV4 address.

> sudo mkdir -p /var/www/200~192.168.0.0/html

Move this directory inside another one that has the IPV4 address as a dicrectory name. 
This way you can organize the content by host machine because NGINX can serve multiple
sites in a single host machine.

** Remember to change the IPV4 192.168.0.0 tou your droplet IPV4 address

> sudo mv /var/www/200~192.168.0.0 /var/www/192.168.0.0

Create your first index page using the following command and changin the IPV4 address to your
droplet IPV4 address:

> sudo vim /var/www/192.168.0.0/html/index.html

### Creating a new server block

The servers blocks are located at /etc/nginx/sites-available

To create a new one type the command:

> vim /etc/nginx/sites-available/<IPV4 address>
>
>> Example vim /etc/nginx/sites-available/192.168.0.0

Inside the editor type the following content changing the IPV4 address 192.168.0.0
 for your doplet IPV4 address:

>
> server {
>        listen 80;
>        listen [::]:80;
>
>        root /var/www/192.168.0.0/html;
>        index index.html;
>        server_name 192.168.0.0;
>
>        location / {
>                try_files $uri $uri/ =404;
>        }
> }
>

Create a soft link to your server block by typing the following command and changing the
IPV4 address 192.168.0.0. to your droplet IPV4 address:

> sudo ln -s /etc/nginx/sites-available/192.168.0.0 /etc/nginx/sites-enabled/

### Testing NGINX configuration

To test if there is no syntax error and if everything is working properly type the following command:

> sudo nginx -t

The result will be syntax is ok amd test is sucessful.

### Restarting the server

Finally to apply all the mofifications is just restart the server

> sudo systemctl restart nginx

## Installing UFW
	
You can ckeck the status of your UFW trough the command:

> sudo ufw status

The availables applications are displayed using the following command:

> sudo ufw app list

- Nginx Full: Opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
- Nginx HTTP: Opens only port 80 (normal, unencrypted web traffic)
- Nginx HTTPS: opens only port 443 (TLS/SSL encrypted traffic)
- OpenSSH

** In our case we selected the NginxHTTP and OpenSSH to just allow conexions with our HTTP by port 80 and SSH conexions.

You can enable this using the following commands:

> sudo ufw allow OpenSSH

> sudo ufw allow 'Nginx HTTP'

### Enabling / Disabling UFW

To enable your firewall type:

> sudo ufw enable

To disble your firewall type:

> sudo ufw diable
	
** Ensure that your are still able to connect to your server using SSH and HTTP.


