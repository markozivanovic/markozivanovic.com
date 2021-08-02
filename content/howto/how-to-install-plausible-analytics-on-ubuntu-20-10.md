+++
author = "Marko Zivanovic"
title = "How to Install Plausible analytics on Ubuntu 20.10"
date = "2021-03-22"
tags = [
    "plausible", "analytics", "ubuntu"
]
categories = [
    "howto", "self-hosting"
]
+++

As consumers are increasingly aware of the privacy issues on the modern web, that trend has opened up space for some interesting privacy-friendly tools to help you charge your business. For many years, Google Analytics was pretty much the only analytics suite worth its salt, but nowadays you can choose from many sound options.

If you want to own your data and be in charge of the tools you’re using, it’s nice to look at some of the alternatives, like <a href="https://plausible.io/" target="_blank">Plausible</a>.

<a href="https://plausible.io/" target="_blank">Plausible</a> is a lightweight and open-source website analytics tool. No cookies and fully compliant with GDPR, CCPA, and PECR. It’s fully open-source, and it’s quite easy to install.

## Goals of the tutorial

You will install the fully operational analytics suite Plausible, ready to embed it into any number of websites to collect data from.


## Prerequisites

- A machine running Ubuntu 20.04 (eg. smallest droplets on DigitalOcean/Vults will suffince).
- A Fully Qualified Domain Name (FQDN) assigned to your server’s IP address.

## Installing required software

As you always should, once logged into the Ubuntu instance – update and upgrade all the packages by running:

{{< highlight bash >}}
$ sudo apt-get update -y
$ sudo apt-get upgrade -y
{{< /highlight >}}

## Installing git and nginx

We will need git, as well as nginx for the later part of this tutorial, so let’s install them also:

{{< highlight bash >}}
$ sudo apt-get install nginx
$ sudo apt-get install git
{{< /highlight >}}

## Installing Docker

Plausible uses many open source packages, but luckily, it makes it easy for us to set it up by using Docker.

To install Docker, we first need to add its official GPG key, and set up the stable repository:

{{< highlight bash >}}
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu focal stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
{{< /highlight >}}

**Note:** You might notice the word focal in the last command, which is the code name of the previous Ubuntu version – Ubuntu 20.04. That’s because the team from Docker doesn’t update their packages immediately after a new distro release. Don’t worry, most of the time everything will work with the packages from the previous Ubuntu version.

Now, we run the update once more, and run the installation of Docker’s packages:

{{< highlight bash >}}
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli docker-compose containerd.io
{{< /highlight >}}

## Cloning Plausible

Navigate to the root nginx directory, clone Plausible, and switch to its directory:

{{< highlight bash >}}
$ cd /var/www
$ git clone https://github.com/plausible/hosting
$ cd hosting
{{< /highlight >}}

## Configuration

Inside the Plausible directory, there is only one file that we will want to edit – **plausible-conf.env**

Inside of the **plausible-conf.env** you will find the following:

{{< highlight bash >}}
ADMIN_USER_EMAIL=replace-me
ADMIN_USER_NAME=replace-me
ADMIN_USER_PWD=replace-me
BASE_URL=replace-me
SECRET_KEY_BASE=replace-me
{{< /highlight >}}

These are the minimum variables that you need to fill for Plausible to work, but we will add some more.

One thing you’ll need to generate is the secret key, which you can do by running the following command, which will output a random string 64 characters long:

{{< highlight bash >}}
$ openssl rand -base64 64
{{< /highlight >}}

Take a look at the example of the fully filled plausible-conf.env. You will need to replace the values shown with your own values:

{{< highlight bash >}}
ADMIN_USER_EMAIL=admin@example.com #replace with your email (should be working one)
ADMIN_USER_NAME=admin #name it anyway you want
ADMIN_USER_PWD=adm1nPassword123 #the stronger, the better
BASE_URL=https://analytics.example.com #this is your URL, use the domain attached to this instance's IP, must start with http/https
SECRET_KEY_BASE=ZN5JhjiQ+TGJP8hMRerTAXtu/7bmd3eZitRaIqlvaQXdkHrODe6wxSLN4izMOPbqpRnTpghI6FJX3Bvf8RVMRQ== #generated key with the above command


MAILER_EMAIL=no-reply@analytics.local #can be anything
SMTP_HOST_ADDR=smtp.google.com #gmail, zoho, whatever you use
SMTP_USER_NAME=myemail@gmail.com #smtp username
SMTP_USER_PWD=smptProviderPassword123 #smpt password
SMTP_RETRIES=3
{{< /highlight >}}

Once you populated your **plausible-conf.env** file, save it with your favorite editor. We are now ready to launch Plausible.
Running Plausible

This is simple, just run:

{{< highlight bash >}}
docker-compose up --detach
{{< /highlight >}}

Before we can open our browser, and check our new shiny analytics suite, we must first take care of our domain and the proxy.
Set up nginx reverse proxy

Open up the following file with your favorite editor, for example nano, like this:

{{< highlight bash >}}
$ sudo nano /etc/nginx/sites-enabled/default
{{< /highlight >}}

Change the contents of the file, so it looks like this:

{{< highlight bash >}}
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name analytics.example.com; #change this to your url

    location / {
            proxy_pass http://127.0.0.1:8000;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
{{< /highlight >}}

Save the file, and restart the nginx service:

{{< highlight bash >}}
$ sudo service nginx restart
{{< /highlight >}}

## Install SSL certificate

Because we added the https prefix in the Plausible configuration file, going to our domain right now will return 502 – Bad Gateway. To make it running, there’s only one step left – installing the SSL certificate.

Fortunately, with LetsEncrypt it’s really easy!

Run the following commands to install certbot:

{{< highlight bash >}}
$ sudo snap install core; sudo snap refresh core
$ sudo snap install --classic certbot
$ sudo ln -s /snap/bin/certbot /usr/bin/certbot
{{< /highlight >}}

After the certbot is installed, run:

{{< highlight bash >}}
$ sudo certbot --nginx
{{< /highlight >}}

You will be presented with a prompt that will ask you to enter your email address, accept the terms and choose the domain (the one you entered in the nginx config will be displayed).

After that is done, let’s restart the nginx once more:

{{< highlight bash >}}
$ sudo service nginx restart
{{< /highlight >}}

## First Login to Plausible

Congratulations! Everything is ready! Open up your browser and navigate to **https[]()://analytics.example.com**.

You will see a login screen. Enter the details you have provided in the Plausible configuration file. After that, you will receive a confirmation email you also typed in there.

If for some reason you don’t receive the email, and you’re sure you entered the correct SMTP settings previously – don’t worry, we can activate the user running the following command in the root directory of Plausible on our server:

{{< highlight bash >}}
$ docker exec hosting_plausible_db_1 psql -U postgres -d plausible_db -c "UPDATE users SET email_verified = true;"
{{< /highlight >}}

You are now ready to use all the features that Plausible offers. For information about using the suite, check out the <a href="https://plausible.io/docs" target="_blank">Plausible official documentation</a>.
