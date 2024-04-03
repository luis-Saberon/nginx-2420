**Unless specified in the instructions, assume all file names and locations should be names as is**

`sudo pacman -Syu`
This will insure that all your base packages are up to date

`sudo pacman -S nginx`
This will install nginx, -S synchronizes packages, and will download any packages nginx depends on.
Nginx could be done as nginx-mainline, which is a newer version. Just nginx is the stable version

`sudo systemctl start nginx.service`
Uses systemctl to start the nginx service

`sudo systemctl enable nginx.service`
Uses systemctl to enable the nginx.service starting on boot. So that if you turn off your server, you don't have to run
the start command again.

`sudo pacman -S vim`
This will install vim, the text editor we will be using to edit files.

`sudo mkdir /etc/nginx/sites-available`
Makes the sites-available directory. This is where we will be storing our server blocks.

`sudo mkdir /etc/nginx/sites-enabled`
Makes the sites-enabled directory. This is to allow easy enabling and disabling of sites, using a symbolic link

`cd /etc/nginx/sites-available``
Move to the sites-available directory

`sudo vim nginx-2420.conf`
This creates and starts editing the server config file. You can name the file whatever you want, as long as it ends in
.conf and it is in the sites-available directory. From now on, whenever you see nginx-2420, replace it with whatever you
choose to name your file.

```bash

server{

    listen 80;

    server_name 24.199.115.90;

    root /web/html/nginx-2420;

    location / {

    index index.html index.htm;

}

}

```

This is called the server block for our server. Listen means that we're listening on port 80, server_name is just the
name of your server, making sure it's consistent with the name given above, or the ip address of your server if you have
no domain name. Root is where we are storing any html files. Root can be in any location. Location is telling what html
file to serve at that url. Save and exit the file.

`sudo vim /etc/nginx/nginx.conf`
Opens the nginx.conf file. Then write `include sites-enabled/*;` at the end of the http block. Save and quit. Should
look something like
```bash
http{
...
include sites-enabled/*
}

```

`sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled/nginx-2420.conf`
This creates the symbolic link between the sites available and the sites enabled directories. To disable the site,
unlink the symbolic link using `unlink /etc/nginx/sites-enabled/nginx-2420.conf.`

`sudo mkdir /web/html/nginx-2420`
Makes the directory where the html files will be stored.

`vim index.html`
Makes and starts editing the index.html file. You can make the file whatever you want, or you can use the html provided
below. Save and quit.

```html

<!DOCTYPE html>

<html lang="en">

<head>

       
    <meta charset="UTF-8">

       
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

        <title>2420</title>

        <style>
        * {

            color: #db4b4b;

            background: #16161e;

        }

        body {

            display: flex;

            align-items: center;

            justify-content: center;

            height: 100vh;

            margin: 0;

        }

        h1 {

            text-align: center;

            font-family: sans-serif;

        }
    </style>

</head>

<body>

        <h1>All your base are belong to us</h1>

</body>

</html>

```

`sudo systemctl reload nginx`
Reload nginx and its config file to use our changes.