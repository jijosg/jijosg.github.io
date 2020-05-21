---
layout: post
title: "Create sudo user in CentOS"
comments: false
redirect_from: "/2013/10/02/sudoaccess/"
permalink: create-sudo-user-centos
---

How to create a user in CentOS and give sudo access
To create a user in CentOS follow these steps

1. You must be logged in as root to add a new user

2. Issue the useradd command to create a locked account
~~~sh
useradd <username>
~~~

3. Issue the passwd command to set the password of the newly created user
~~~sh
passwd <username>
~~~

This will you prompt you to enter the password of the newly created user

4. To give the user sudo access you need to add  the user to wheel group 

To do this issue the command : `visudo`

To be able to add a user to the wheel group you must uncomment the line ie
~~~sh
#%wheel   ALL=(ALL)   ALL
~~~ 
to
~~~sh
%wheel   ALL=(ALL)   ALL
~~~

5. Adding the newly created user to wheel group
~~~sh
usermod -G wheel  <username>
~~~

6. Finished  - Your user now has sudo access !! enjoy !!