---
layout: post
author: opcoder0
---

## How To Setup ReviewBoard on Ubuntu

This post provides a series of steps and commands that is required to setup and configure reviewboard server on your Ubuntu OS. Having reviewboard to post and track changes and reviews is really awesome. Well of-course reviewboard documentation has done a wonderful job of describing everything needed. I had to open a few other sites (like MySQL and some forums) to complete the steps successfully. I stumbled upon a few errors while I was setting up reviewboard and I’ve added steps on how I got around them and got it up and running.

I have a Ubuntu 10.10 running on a system on which I’ve setup reviewboard. You could put these in a script and it would work –

There are some pre-requisites –

```
sudo apt-get install firefox
sudo apt-get install mysql-server
sudo apt-get install libmysqlclient-dev
sudo apt-get install apache2
sudo apt-get install libapache2-mod-python
sudo apt-get install python-setuptools
sudo apt-get install python-dev  # (re-starts the machine to complete setup)
sudo apt-get install memcached
sudo apt-get install patch
```

All of reviewboard components and python modules can be installed using setuptools (easy_install)

```
sudo easy_install python-memcached
sudo easy_install ReviewBoard
sudo easy_install mysql-python
sudo easy_install python-mysqldb
```

Am setting this up for subversion. Installing subversion and the python-svn (source control components)

```
sudo apt-get install subversion
sudo apt-get install python-svn
```

In order to setup / configure reviewboard you’ll need a MySQL database (I’ve chosen MySQL, you could choose others listed on reviewboard site). So run the following command –

`mysql –user=root –password=”your passwd here” mysql`

Create the reviewboard database of your choice. At the mysql prompt type

```
mysql> CREATE DATABASE {your-database-name-here};
```

At the mysql prompt create a reviewboard user and grant it all permissions.

```
mysql> CREATE USER ‘rbuser’@’localhost’ IDENTIFIED BY ‘your_password’;
mysql> GRANT ALL PRIVILEGES ON *.* TO ‘rbuser’@’localhost’ WITH GRANT OPTION;
```

Now run the configuration command rb-site install to configure your review board server

```
sudo rb-site install /var/www/your.review-site.com
sudo chown -R www-data:www-data /var/www/your.review-site.com
```

`rb-site install` is going to ask you a lot of questions, which you should be able to answer since it would be your choices and specific to your environment. Remember your reviewboard admin credentials which will be required later.

**NOTE:** Apache runs as `www-data` user on my Ubuntu. If you don’t change the owner and group of `/var/www/your.review-site.com` to `www-data:www-data`, Apache won’t have the right permissions to add the source control repository. This is required because, you would have to upload your changed files and reviewboard should be able to detect the difference between what is in the source repository vs. the file you’ve submitted.

```
cd /etc/apache2/sites-available
cp /var/www/your.review-site.com/conf/apache-modpython.conf your.review-site.com.conf
cd ../sites-enabled
sudo ln -s ../sites-available/your.review-site.com.conf .
```

And you’re done !

With this, you have a review board setup. Based on the questions you’ve answered (hopefully with correct details) visit reviewboard link http://<your.rb.hostname&gt;:<port>/dashboard

As an admin you’ll have to add your source control repository URL in to your reviewboard environment. Login to your reviewboard server as ‘admin’. Click on the “Add” link next to “Repositories” in the “Database” tab. In this window, fill-up the Source control details. And click ‘Save’.

**NOTE-1:** If you’ve forgotten to run the chown command command above to set the owner to www-data on /var/www/your.review-site.com/ reviewboard cannot create the configuration.

**NOTE-2:** If you get an error saying unable to create .subversion directory under /var/www/your.review-site.com/data, then create it manually.

Once you’ve added your source repository successfully, the setup is complete. Users of reviewboard need to create their accounts and login to reviewboard. To post new reviews, users would need post-review (part of RBTools). This is a tool that allows you to post review from your source control using command line. This tool is available for Linux, Windows and MacOS. This can be installed by running `easy_install -U RBTools`

For users to start using the tool (in case of SVN). In your SVN checked-out directory set a property using –

```
svn propset reviewboard:url http://<your.rb.hostname&gt;:<port>/
post-review –user=<your-username> –password=<your-passwd> -p
```

This should post this for review !
