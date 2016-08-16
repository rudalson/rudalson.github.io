# 한글 설치
* sudo apt purge ibus
* sudo apt install fcitx-hangul
* sudo apt install fcitx
* sudo apt install language-selector-gnome

language-selector-gnome 가셔서, 입력기를 fcitx 로 변경

윈도우키 누르시고 -> fcitx 환경설정 -> Global Config -> Trigger input method 에서 Hangul 을 선택하시고 원하는 키 조합을 누르세요


# google chrome 설치
sudo apt install chromium-browser

# lxd 설치
sudo apt install lxd
sudo lxd init

dev@vm-ubuntux64:~$ sudo lxd init
Name of the storage backend to use (dir or zfs): 
Invalid input, try again.

Name of the storage backend to use (dir or zfs): dir
Would you like LXD to be available over the network (yes/no)? yes
Address to bind LXD to (not including port):  
Invalid input, try again.

Address to bind LXD to (not including port): 0.0.0.0
Port to bind LXD to (8443 recommended): 8443
Trust password for new clients: 
Again: 
Do you want to configure the LXD bridge (yes/no)? yes
Warning: Stopping lxd.service, but it can still be activated by:
  lxd.socket
LXD has been successfully configured.

dev@vm-ubuntux64:~$ lxc remote list
Generating a client certificate. This may take a minute...
If this is your first time using LXD, you should also run: sudo lxd init

+-----------------+------------------------------------------+---------------+--------+--------+
|      NAME       |                   URL                    |   PROTOCOL    | PUBLIC | STATIC |
+-----------------+------------------------------------------+---------------+--------+--------+
| images          | https://images.linuxcontainers.org       | lxd           | YES    | NO     |
+-----------------+------------------------------------------+---------------+--------+--------+
| local (default) | unix://                                  | lxd           | NO     | YES    |
+-----------------+------------------------------------------+---------------+--------+--------+
| ubuntu          | https://cloud-images.ubuntu.com/releases | simplestreams | YES    | YES    |
+-----------------+------------------------------------------+---------------+--------+--------+
| ubuntu-daily    | https://cloud-images.ubuntu.com/daily    | simplestreams | YES    | YES    |
+-----------------+------------------------------------------+---------------+--------+--------+



dev@vm-ubuntux64:~$ lxc image list images:
+---------------------------------+--------------+--------+-------------------------------------------+---------+----------+-------------------------------+
|              ALIAS              | FINGERPRINT  | PUBLIC |                DESCRIPTION                |  ARCH   |   SIZE   |          UPLOAD DATE          |
+---------------------------------+--------------+--------+-------------------------------------------+---------+----------+-------------------------------+
| alpine/3.0/amd64 (1 more)       | b907c9e6167f | yes    | Alpine 3.0 (amd64) (20160501_17:50)       | x86_64  | 2.09MB   | May 1, 2016 at 6:15pm (UTC)   |
+---------------------------------+--------------+--------+-------------------------------------------+---------+----------+-------------------------------+
| alpine/3.0/i386 (1 more)        | 9930c157484f | yes    | Alpine 3.0 (i386) (20160501_17:50)        | i686    | 2.06MB   | May 1, 2016 at 6:15pm (UTC)   |
+---------------------------------+--------------+--------+-------------------------------------------+---------+----------+-------------------------------+
| alpine/3.1/amd64 (1 more)       | 641f1d4b7838 | yes    | Alpine 3.1 (amd64) (20160501_17:50)       | x86_64  | 2.32MB   | May 1, 2016 at 6:15pm (UTC)   |
+---------------------------------+--------------+--------+-------------------------------------------+---------+----------+-------------------------------+
| alpine/3.1/i386 (1 more)        | 982fafb37cdb | yes    | Alpine 3.1 (i386) (20160501_17:50)        | i686    | 2.26MB   | May 1, 2016 at 6:15pm (UTC)   |
+---------------------------------+--------------+--------+-------------------------------------------+---------+----------+-------------------------------+
| alpine/3.2/amd64 (1 more)       | a7d7335e7a2f | yes    | Alpine 3.2 (amd64) (20160501_17:50)       | x86_64  | 2.61MB   | May 1, 2016 at 6:15pm (UTC)   |
+---------------------------------+--------------+--------+-------------------------------------------+---------+----------+-------------------------------+
| alpine/3.2/i386 (1 more)        | ec99f4fefdab | yes    | Alpine 3.2 (i386) (20160501_17:50)        | i686    | 2.44MB   | May 1, 2016 at 6:15pm (UTC)   |
+---------------------------------+--------------+--------+-------------------------------------------+---------+----------+-------------------------------+
| alpine/3.3/amd64 (1 more)       | 0ed6f11f9095 | yes    | Alpine 3.3 (amd64) (20160501_17:50)       | x86_64  | 2.61MB   | May 1, 2016 at 6:15pm (UTC)   |
+---------------------------------+--------------+--------+-------------------------------------------+---------+----------+-------------------------------+
...


dev@vm-ubuntux64:~$ sudo lxc init local:9e5c85eb9c24 test-con1
Creating test-con1
dev@vm-ubuntux64:~$ sudo lxc list
+-------------------+---------+------+------+------------+-----------+
|       NAME        |  STATE  | IPV4 | IPV6 |    TYPE    | SNAPSHOTS |
+-------------------+---------+------+------+------------+-----------+
| test-con1         | STOPPED |      |      | PERSISTENT | 0         |
+-------------------+---------+------+------+------------+-----------+

dev@vm-ubuntux64:~$ sudo lxc start test-con1
dev@vm-ubuntux64:~$ sudo lxc list
+-------------------+---------+------+----------------------------------------------+------------+-----------+
|       NAME        |  STATE  | IPV4 |                     IPV6                     |    TYPE    | SNAPSHOTS |
+-------------------+---------+------+----------------------------------------------+------------+-----------+
| test-con1         | RUNNING |      | fd87:953c:dd3a:f380:216:3eff:feeb:a75 (eth0) | PERSISTENT | 0         |
+-------------------+---------+------+----------------------------------------------+------------+-----------+

dev@vm-ubuntux64:~$ sudo lxc exec test-con1 bash
root@test-con1:~# 
root@test-con1:~# sudo apt update
...
root@test-con1:~# sudo apt install openssh-server bash-completion command-not-found byobu git man info aptitude
사람 구실은 할 정도

root@test-con1:~# sudo deluser ubuntu
Removing user `ubuntu' ...
Warning: group `ubuntu' has no more members.
Done.
root@test-con1:~# adduser rudalson   
Adding user `rudalson' ...
Adding new group `rudalson' (1000) ...
Adding new user `rudalson' (1000) with group `rudalson' ...
Creating home directory `/home/rudalson' ...
Copying files from `/etc/skel' ...
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
Changing the user information for rudalson
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] y
root@test-con1:~# 
root@test-con1:~# sudo usermod -G sudo -a rudalson
root@test-con1:~# exit

빠져 나온 후
dev@vm-ubuntux64:~$ sudo lxc stop test-con1
dev@vm-ubuntux64:~$ sudo lxc list
+-------------------+---------+------+------+------------+-----------+
|       NAME        |  STATE  | IPV4 | IPV6 |    TYPE    | SNAPSHOTS |
+-------------------+---------+------+------+------------+-----------+
| test-con1         | STOPPED |      |      | PERSISTENT | 0         |
+-------------------+---------+------+------+------------+-----------+


dev@vm-ubuntux64:~$ sudo lxc start test-con1
dev@vm-ubuntux64:~$ sudo lxc list
+-----------+---------+-----------------------+----------------------------------------------+------------+-----------+
|   NAME    |  STATE  |         IPV4          |                     IPV6                     |    TYPE    | SNAPSHOTS |
+-----------+---------+-----------------------+----------------------------------------------+------------+-----------+
| test-con1 | RUNNING | 10.243.148.229 (eth0) | fd87:953c:dd3a:f380:216:3eff:feeb:a75 (eth0) | PERSISTENT | 0         |
+-----------+---------+-----------------------+----------------------------------------------+------------+-----------+

dev@vm-ubuntux64:~$ ssh rudalson@10.243.148.229
The authenticity of host '10.243.148.229 (10.243.148.229)' can't be established.
ECDSA key fingerprint is SHA256:UH+8d5Fsqb1lV6PZAxN8Kw7eLYLGI4QXvYRax013OKg.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.243.148.229' (ECDSA) to the list of known hosts.
rudalson@10.243.148.229's password: 
Welcome to Ubuntu 15.10 (GNU/Linux 4.4.0-21-generic i686)

 * Documentation:  https://help.ubuntu.com/

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

rudalson@test-con1:~$

dev@vm-ubuntux64:~$ lxc --help
Usage: lxc [subcommand] [options]
Available commands:
	config     - Manage configuration.
	copy       - Copy containers within or in between lxd instances.
	delete     - Delete containers or container snapshots.
	exec       - Execute the specified command in a container.
	file       - Manage files on a container.
	help       - Presents details on how to use LXD.
	image      - Manipulate container images.
	info       - List information on LXD servers and containers.
	launch     - Launch a container from a particular image.
	list       - Lists the available resources.
	move       - Move containers within or in between lxd instances.
	profile    - Manage configuration profiles.
	publish    - Publish containers as images.
	remote     - Manage remote LXD servers.
	restart    - Changes state of one or more containers to restart.
	restore    - Set the current state of a resource back to a snapshot.
	snapshot   - Create a read-only snapshot of a container.
	start      - Changes state of one or more containers to start.
	stop       - Changes state of one or more containers to stop.
	version    - Prints the version number of this client tool.

Options:
  --all              Print less common commands.
  --debug            Print debug information.
  --verbose          Print verbose information.

Environment:
  LXD_CONF           Path to an alternate client configuration directory.
  LXD_DIR            Path to an alternate server directory.




# ZFS 설치 패키지
sudo apt install zfs-initramfs zfs-zed zfsutils-linux zfs-doc

jehos@MacBuntu:~$ lsmod | grep zfs
zfs                  2813952  3
zunicode              331776  1 zfs
zcommon                57344  1 zfs
znvpair                90112  2 zfs,zcommon
spl                   102400  3 zfs,zcommon,znvpair
zavl                   16384  1 zfs


# 추가 드라이버(?)
윈도우키 누르고 -> driver 검색하시면, 독점 드라이버 라는 항목 설치

# snapshot
lxc snapshot --help
sudo lxc snapshot test-con1 snap0
sudo lxc restore ubuntu-vm snap0
