---
layout: post
title: HTTP to HTTPS With LetsEncrypt (Django, Nginx)
---

#### Step 1 Install letsencrypt:
{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install letsencrypt
{% endhighlight %}

#### Step 2: Obtain a SSL certificate:
{% highlight bash %}
$ sudo letsencrypt certonly -a standalone
{% endhighlight %}

#### Step 3: Configure the Nginx server:
My conf. file looks like the following:
{% highlight bash %}
server {
    client_max_body_size 4M;
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name yourwebsite.com www.yourwebsite.com;
    return 301 https://$server_name$request_uri;
}
server { 
    listen 443 ssl http2 default_server;
    listen[::]:443 ssl http2 default_server;
    ssl_certificate /etc/letsencrypt/live/yoursite/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yoursite/privkey.pem;
    ssl_session_cache shared:SSL:50m;
    ssl_stapling_on;
    ssl_stapling_verify on;
    ssl_session_timeout 1d;
    ssl_ciphers: 'you-can-get-your-cipher-at-[mozilla ssl configuration][8]';
    ssl_protocols TLSv1.2;
    ssl_prefer_server_ciphers on;

    add_header Strict-Transport-Security max-age=15768000;
    -----------REST-OF-CONFIGURE UNDERNEATH--------
{% endhighlight %}
Fill ```ssl-ciphers``` according to https://mozilla.github.io/server-side-tls/ssl-config-generator/

#### Step 4 (Recommended): Firewall Permissions:
{% highlight bash %}
$ sudo ufw allow 'Nginx Full'
$ sudo ufw 
$ sudo ufw delete allow 'Nginx HTTP'
{% endhighlight %}

Check nginx configuration:
{% highlight bash %}
$ sudo nginx -t
{% endhighlight %}

Restart the forward proxy and the nginx proxy system:
{% highlight bash %}
$ sudo systemctl restart nginx
{% endhighlight %}

#### Step 5 (Optional): Renew certificate:
{% highlight bash %}
$ sudo systemctl stop nginx
$ sudo letsencrypt renew
$ sudo systemctl restart nginx
{% endhighlight %}


#### Reference:
* [HTTP to HTTPS With LetsEncrypt (Using Django, Nginx, & Gunicorn)](https://spencertechconsulting.com/posts/HTTP-TO-HTTPS/)