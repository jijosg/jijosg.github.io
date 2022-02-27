---
title: "Unable to install R packages rgl & qpcR in sparkR"
---

If you encounter such an error while installing the package rgl in sparkR
~~~sh
configure: using libpng dynamic linkage checking for X... no 
configure: error: X11 not found but required, 
configure aborted.
~~~

its because X11 is a windows library and to resolve use:
Ubuntu : 
{% include codeHeader.html %}
~~~sh
sudo apt-get install libglu1-mesa-dev
~~~

Redhat:
{% include codeHeader.html %}
~~~sh
sudo yum install mesa-libGL-devel mesa-libGLU-devel libpng-devel
~~~
