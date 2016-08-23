---
layout: post
title:  "우분투 16.04 에서 git 버전 업데이트 하기"
date:   2016-08-18 17:16:28 +0900
categories: tool
tags: git ubuntu xerial
---
문득 현재 사용하고 있는 Ubuntu내의 git을 update 하는 방법이 궁금해졌다.
우분투 업데이트 사항들을 비교적 자주 update 해주긴 하지만 이런 것은 변화가 없기 때문이다.

* [Upgrading Ubuntu to Use the Latest Git Version](http://lifeonubuntu.com/upgrading-ubuntu-to-use-the-latest-git-version/)
* [How can I update to a newer version of Git using apt-get?](http://unix.stackexchange.com/questions/33617/how-can-i-update-to-a-newer-version-of-git-using-apt-get)

의 글에서 해답을 찾았고 현재 Ubuntu 16.04 에서도 잘 동작하는 것을 확인하였다.

{% highlight bash %}
$ git --version
git version 2.7.4
{% endhighlight %}
먼저 현재의 git 버전을 확인해 보았다. 버전이 많이 낮은 것은 아니지만....

{% highlight bash %}
$ sudo add-apt-repository ppa:git-core/ppa
 The most current stable version of Git for Ubuntu.

For release candidates, go to https://launchpad.net/~git-core/+archive/candidate .
 More info: https://launchpad.net/~git-core/+archive/ubuntu/ppa
Press [ENTER] to continue or ctrl-c to cancel adding it

gpg: keyring `/tmp/tmp7y8mqwgk/secring.gpg' created
gpg: keyring `/tmp/tmp7y8mqwgk/pubring.gpg' created
gpg: requesting key E1DF1F24 from hkp server keyserver.ubuntu.com
gpg: /tmp/tmp7y8mqwgk/trustdb.gpg: trustdb created
gpg: key E1DF1F24: public key "Launchpad PPA for Ubuntu Git Maintainers" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
OK
$ sudo apt update
Hit:1 http://kr.archive.ubuntu.com/ubuntu xenial InRelease
Get:2 http://kr.archive.ubuntu.com/ubuntu xenial-updates InRelease [95.7 kB]                             
Hit:3 http://kr.archive.ubuntu.com/ubuntu xenial-backports InRelease                                     
Get:4 http://kr.archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages [371 kB]                    
Get:5 http://kr.archive.ubuntu.com/ubuntu xenial-updates/main i386 Packages [367 kB]                     
Get:6 http://security.ubuntu.com/ubuntu xenial-security InRelease [94.5 kB]                         
Get:7 http://kr.archive.ubuntu.com/ubuntu xenial-updates/main Translation-en [140 kB]    
Get:8 http://kr.archive.ubuntu.com/ubuntu xenial-updates/universe amd64 Packages [319 kB]                
Get:9 http://ppa.launchpad.net/git-core/ppa/ubuntu xenial InRelease [17.5 kB]                            
Get:10 http://kr.archive.ubuntu.com/ubuntu xenial-updates/universe i386 Packages [316 kB]                
Get:11 http://kr.archive.ubuntu.com/ubuntu xenial-updates/universe Translation-en [108 kB]               
Get:12 http://ppa.launchpad.net/git-core/ppa/ubuntu xenial/main amd64 Packages [3,212 B]                 
Get:13 http://ppa.launchpad.net/git-core/ppa/ubuntu xenial/main i386 Packages [3,212 B]
Get:14 http://ppa.launchpad.net/git-core/ppa/ubuntu xenial/main Translation-en [2,496 B]
Fetched 1,838 kB in 1s (965 kB/s)                                    
Reading package lists... Done
Building dependency tree       
Reading state information... Done
4 packages can be upgraded. Run 'apt list --upgradable' to see them.
{% endhighlight %}
새 버전을 위해서 git을 위한 새로운 repository를 갱신한 다음 update를 해준다.

{% highlight bash %}
$ sudo apt install git
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  linux-headers-4.4.0-21 linux-headers-4.4.0-21-generic linux-headers-4.4.0-22
  linux-headers-4.4.0-22-generic linux-headers-4.4.0-24 linux-headers-4.4.0-24-generic
  linux-headers-4.4.0-28 linux-headers-4.4.0-28-generic linux-image-4.4.0-21-generic
  linux-image-4.4.0-22-generic linux-image-4.4.0-24-generic linux-image-4.4.0-28-generic
  linux-image-extra-4.4.0-21-generic linux-image-extra-4.4.0-22-generic
  linux-image-extra-4.4.0-24-generic linux-image-extra-4.4.0-28-generic
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  git-man gitk
Suggested packages:
  git-daemon-run | git-daemon-sysvinit git-doc git-el git-email git-gui gitweb git-arch git-cvs
  git-mediawiki git-svn
The following packages will be upgraded:
  git git-man gitk
3 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
Need to get 6,317 kB of archives.
After this operation, 3,164 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://ppa.launchpad.net/git-core/ppa/ubuntu xenial/main amd64 gitk all 1:2.9.3-0ppa1~ubuntu16.04.1 [776 kB]
Get:2 http://ppa.launchpad.net/git-core/ppa/ubuntu xenial/main amd64 git amd64 1:2.9.3-0ppa1~ubuntu16.04.1 [4,148 kB]
Get:3 http://ppa.launchpad.net/git-core/ppa/ubuntu xenial/main amd64 git-man all 1:2.9.3-0ppa1~ubuntu16.04.1 [1,393 kB]
Fetched 6,317 kB in 11s (563 kB/s)                                                                       
(Reading database ... 332838 files and directories currently installed.)
Preparing to unpack .../gitk_1%3a2.9.3-0ppa1~ubuntu16.04.1_all.deb ...
Unpacking gitk (1:2.9.3-0ppa1~ubuntu16.04.1) over (1:2.7.4-0ubuntu1) ...
Preparing to unpack .../git_1%3a2.9.3-0ppa1~ubuntu16.04.1_amd64.deb ...
Unpacking git (1:2.9.3-0ppa1~ubuntu16.04.1) over (1:2.7.4-0ubuntu1) ...
Preparing to unpack .../git-man_1%3a2.9.3-0ppa1~ubuntu16.04.1_all.deb ...
Unpacking git-man (1:2.9.3-0ppa1~ubuntu16.04.1) over (1:2.7.4-0ubuntu1) ...
Processing triggers for man-db (2.7.5-1) ...
Setting up git-man (1:2.9.3-0ppa1~ubuntu16.04.1) ...
Setting up git (1:2.9.3-0ppa1~ubuntu16.04.1) ...
Setting up gitk (1:2.9.3-0ppa1~ubuntu16.04.1) ...
{% endhighlight %}

{% highlight bash %}
$ git --version
git version 2.9.3
{% endhighlight %}
이제 버전을 확인해 보면 잘 되는 것을 알 수 있다.
