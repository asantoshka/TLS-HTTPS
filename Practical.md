So before starting with this activity, we need to setup few things. Below are achievable with simple google search.

1. Virtual box 
2. Ubuntu machine in virtual box 
TODO:

Rest we will do on the go.

What we are going to do:
1. Installing Nginx web server on the Ubuntu host.
2. Enabling SSL on it.
TODO:

We will be performing all testing related to the webpage  from the host machine, so we need to keep few things in mind.

1. The Ubuntu Network interface is configured with bridged adapter.
2. Need to add the ubuntu ip to the host file of the host machine.

First we will install the Nginx on our ubuntu host. To do so we will run the below command.

```
sudo apt update
sudo apt install nginx
```

Once the Nginx is installed, we need to allow the required services on the ufw (Uncomplicated firewall). Before allowing them, we will check what all applications are configured on it.

```
sudo ufw app status
```
Output:

```
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```
* Nginx Full: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
* Nginx HTTP: This profile opens only port 80 (normal, unencrypted web traffic)
* Nginx HTTPS: This profile opens only port 443 (TLS/SSL encrypted traffic)

We will allow both HTTP and HTTPS so we will run the below command.
```
sudo ufw allow 'Nginx Full'
```

Now we can verify the changes by using below command.
```
sudo ufw status
```
Output:
```
Status: active

To                         Action      From
--                         ------      ----
Nginx Full                 ALLOW       Anywhere                  
Nginx Full (v6)            ALLOW       Anywhere (v6)  
```
Now we can check with the systemd init system to make sure the service is running

```
systemctl status nginx
```
It will give an output where the status shows `active (running)`.

Now our server is up and running. Now to manage the server, we will be using the following commands.

To stop your web server, type:

```
sudo systemctl stop nginx
```
To start the web server when it is stopped, type:

```
sudo systemctl start nginx
```

To stop and then start the service again, type:

```
sudo systemctl restart nginx
```

If you are simply making configuration changes, Nginx can often reload without dropping connections. To do this, type:

```
sudo systemctl reload nginx
```

By default, Nginx is configured to start automatically when the server boots. If this is not what you want, you can disable this behavior by typing:

```
sudo systemctl disable nginx
```

To re-enable the service to start up at boot, you can type:

```
sudo systemctl enable nginx
```

So we will set up server block. i.e for a specific domain. We can host multiple sub domains on a single nginx server. Here, we will setup a domain with name `asantoshka.online`.

Create the directory for asantoshka.online as follows, using the -p flag to create any necessary parent directories:

```
sudo mkdir -p /var/www/asantoshka.online/html
```
Next, assign ownership of the directory with the $USER environment variable:

```
sudo chown -R $USER:$USER /var/www/asantoshka.online/html
```
The permissions of your web roots should be correct if you haven’t modified your umask value, but you can make sure by typing:

```
sudo chmod -R 755 /var/www/    
```
Next, create a sample index.html page using nano or your favorite editor:

```
nano /var/www/asantoshka.online/html/index.html
```
Inside, add the following sample HTML:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Santosh's Blog</title>
</head>
<body>
    <h2>Welcome to my Blog ;)</h2>
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Necessitatibus voluptatum sunt perspiciatis voluptates vero nesciunt amet provident. Explicabo, nulla assumenda blanditiis quasi aut, iste ratione vitae autem, hic veniam sapiente.
    Lorem ipsum dolor sit, amet consectetur adipisicing elit. Inventore hic sequi voluptatem ad, voluptas vel quibusdam amet maxime soluta obcaecati veritatis blanditiis temporibus alias officia dolorum natus laboriosam illo. Aperiam.
    <h1>Thank you</h1>
    
</body>
</html>
```

Save and close the file when you are finished.

In order for Nginx to serve this content, it’s necessary to create a server block with the correct directives. Instead of modifying the default configuration file directly, let’s make a new one at /etc/nginx/sites-available/asantoshka.online:

```
sudo nano /etc/nginx/sites-available/asantoshka.online
```
Paste in the following configuration block, which is similar to the default, but updated for our new directory and domain name:
/etc/nginx/sites-available/asantoshka.online

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/asantoshka.online/html;
        index index.html index.htm index.nginx-debian.html;

        server_name asantoshka.online www.asantoshka.online;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

Notice that we’ve updated the root configuration to our new directory, and the server_name to our domain name.

Next, let’s enable the file by creating a link from it to the sites-enabled directory, which Nginx reads from during startup:

```
sudo ln -s /etc/nginx/sites-available/asantoshka.online /etc/nginx/sites-enabled/
```
Next, test to make sure that there are no syntax errors in any of your Nginx files:

```
sudo nginx -t
```
If there aren’t any problems, restart Nginx to enable your changes:

```
sudo systemctl restart nginx
```
Now we can try accessing http://asantoshka.online

Now make the communication secure by enabling TLS on the server. 

### Generating a SSL certificate and configuring it to the web server:

To generate the certificate we need to go to /etc/ssl directory and run the below command.

```
cd /etc/ssl
openssl req -x509 -newkey rsa:4096 -keyout asantoshka.online.key -out asantoshka.online.pem -days 365
```
Output:
```
root@samTerminal:/etc/ssl# openssl req -x509 -newkey rsa:4096 -keyout asantoshka.online.key -out asantoshka.online.pem -days 365
Generating a RSA private key
...................................++++
.++++
writing new private key to 'asantoshka.online.key'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:California
Locality Name (eg, city) []:San Francisco
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Santosh    
Organizational Unit Name (eg, section) []:Blog
Common Name (e.g. server FQDN or YOUR name) []:asantoshka.online
Email Address []:asantoshka.outlook.com
```

It will ask for PEM passphrase, using which the private key will be encrypted.

Once the certificate and the keys are generated we can view them in the /etc/ssl directory.

```
-rw-------   1 root root      3414 Sep  5 13:11 asantoshka.online.key
-rw-r--r--   1 root root      2183 Sep  5 13:12 asantoshka.online.pem
```

We have to add a file with name `passphrase.pass` in the same directory containing the passphrase of the private key.

Now we will enable the HTTPS on the nginx web server by adding below code to the `/etc/nginx/sites-available/asantoshka.online` file.

```
server {
        listen 443;
        listen [::]:443;

        ssl    on;
        ssl_certificate    /etc/ssl/asantoshka.online.pem; 
        ssl_certificate_key    /etc/ssl/asantoshka.online.key;
        ssl_password_file	/etc/ssl/passphrase.pass;	

        root /var/www/asantoshka.online/html;
        index index.html index.htm index.nginx-debian.html;

        server_name asantoshka.online www.asantoshka.online;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

>We can avoid adding "ssl_password_file" option to configuration of the Nginx server by creating private key without encryption.

Now we can restart the nginx server by using `systemctl restart nginx` command and try to acces the https://asantoshka.online.


https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04


