---
layout: post
title:  "OEL(Oracle Enterprise Linux)에 drbd 8.4.5 설치"
categories: system
tags: drbd oel linux
---
{% highlight bash %}
yum -y install automake gcc libxslt make wget rpm-build flex kernel-devel kernel-uek-devel
cd
mkdir pkg
cd pkg
wget http://oss.linbit.com/drbd/drbd-utils-8.9.2.tar.gz
wget http://oss.linbit.com/drbd/8.4/drbd-8.4.5.tar.gz
tar xvfz drbd-8.4.5.tar.gz
tar xvfz drbd-utils-8.9.2.tar.gz
cd drbd-utils-8.9.2
./configure --sysconfdir=/etc --localstatedir=/var
make && make install
cd ..
cd drbd-8.4.5
make KDIR=/usr/src/kernels/2.6.32-642.4.2.el6.x86_64/
make install
{% endhighlight %}
