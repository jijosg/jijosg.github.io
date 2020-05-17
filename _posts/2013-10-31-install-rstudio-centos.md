---
layout: post
title: Install RStudio in CentOS 6
comments: false
redirect_from: "/2013/10/31/installcentos/"
permalink: install-rstudio-in-centos6
---

Centos doesnot support R studio desktop so you would have to install r studio server instead which works in a browser 


#### For EL6


RStudio Server has several dependencies on packages (including R itself) found in the Extra Packages for Enterprise Linux (EPEL) repository. If you don't already have this repository available you should add it to your system using the instructions found on the Fedora EPEL website.

`$ su -c 'rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm'`


After enabling EPEL you should then ensure that you have installed the version of R available from EPEL. You can do this using the following command:
 
`$ sudo yum install R` 
 
 
#### Download and Install
To download and install RStudio Server open a terminal window and execute the commands corresponding to the 32 or 64-bit version as appropriate.
>32-bit Size: 17.5 MB MD5: 3bc83db8c23c212c391342e731f65823
`$ wget http://download2.rstudio.org/rstudio-server-0.97.551-i686.rpm $ sudo yum install --nogpgcheck rstudio-server-0.97.551-i686.rpm`

>64-bit Size: 17.6 MB MD5: c89d5574a587472d06f72b304356a776

`$ wget http://download2.rstudio.org/rstudio-server-0.97.551-x86_64.rpm $ sudo yum install --nogpgcheck rstudio-server-0.97.551-x86_64.rpm`


Then in your browser go to address

`http://<your_server_name>:8787`

The login credentials are the current username and password of your centos account 
 

[Reference link](http://www.rstudio.com/ide/download/server)