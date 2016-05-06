---
layout: post
title:  "Install Drupal 8 on Ubuntu 16.04"
date:   2016-05-06 17:19:26 +0530
categories: drupal-8 ubuntu-16.04
---

WordPress has their famous 5-minute install. It's so fast, that it takes more
time to read a manual on how to set it up, than to actually set it up. So, let's
see how to install Drupal instead. These two are both content management systems
that is much better than managing hundreds of HTML files alone.

#### 1. Install Ubuntu, Apache, MySQL, PHP

Brennen Bearnes has written a [nice detailed article][digital-ocean] on that
already. Go check that out.

Once done, let's make life simpler with the filesystem. it's so much easier to
work without root permission that's required to access `/var/www/`. Link up
Apache's default directory to a `public_html` folder in your home like so
{% highlight bash %}
sudo mv /var/www/html /var/www/html.bkp
mkdir ~/public_html
sudo ln -s ~/public_html /var/www/html
{% endhighlight %}

You will also need the following modules enabled:

* PHP modules
{% highlight bash %}
sudo apt install php-mysql php-xml php-gd php-json php-curl
{% endhighlight %}

* Apache Rewrite module
{% highlight bash %}
a2enmod rewrite
sudo service apache2 restart
{% endhighlight %}

* Allow Apache to use `.htaccess` files. Change `AllowOverride None` to read
`AllowOverride All`.
{% highlight bash %}
sudo nano /etc/apache2/apache2.conf
{% endhighlight %}
{% highlight xml %}
<Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
{% endhighlight %}

Save the file with `Ctrl+X` and then `Y`. Finally, restart the Apache server to
reflect the changes.


#### 2. Download Drupal 8

…from [their site](https://www.drupal.org/8/download). Extract the archive to the
`public_html` directory within your home folder. Also copy
`sites/default/default.settings.php` to `sites/default/settings.php`.

Give them enough permissions for Apache to work with it. But making sure you can
write to it as well. (Replace `me` with your username.)
{% highlight bash %}
sudo chown --recursive me:www-data ~/public_html
sudo chmod --recursive g+w ~/public_html
{% endhighlight %}

Open the URL `http://localhost` on your browser, to start the installation.


#### 3. Create MySQL database

Log into MySQL server console.
{% highlight bash %}
mysql -u root -p
{% endhighlight %}

Create a databse, and a user for the Drupal website.

{% highlight sql %}
CREATE DATABASE drupal;
CREATE USER 'drupal' IDENTIFIED BY 'drupal';
GRANT ALL ON drupal.* TO 'drupal';
{% endhighlight %}

You can then use these credentials in the installation setup.

#### 4. Finishing Up

After all the Drupal modules have been installed, you will be asked to enter the
site name, email address, admin login, etc.

Hurray!~ You're done with the basic setup. That was way more than 5 minutes… 😉
You might want to read the next post on how to start theming the site.



​

Thank you for scrolling to the bottom of this page! Do let me know
how it was.

*******

[digital-ocean]: https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04
