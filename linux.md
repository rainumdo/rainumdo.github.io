# 100

```
git checkout master 
git checkout commit  file/dir
```

# 99

bluetooth

```
scan on
devices
pair <DEVICE_MAC_ADDRESS>
connect <DEVICE_MAC_ADDRESS>
trust <DEVICE_MAC_ADDRESS>

sudo rfcomm connect hci0 98:D3:02:96:9A:BB
```

# 98

`<leader>sk`, search for keymaps in nvim

# 97

add ppa and rm ppa

```
sudo add-apt-repository ppa:user/ppa_name
ls /etc/apt/sources.list.d
sudo rm -i /etc/apt/sources.list.d/ppa_name.list
```

# 96

obs-studio

```shell
sudo add-apt-repository ppa:obsproject/obs-studio
sudo apt update
sudo apt install obs-studio
```

# 95

docsify

```shell
npm i docsify-cli -g
mkdir docs
docsify init docs
docsify serve docs
```

# 94

download and overwrite

```shell
https://github.com/OmniSharp/omnisharp-roslyn/releases
/home/rainumdo/.config/coc/extensions/node_modules/coc-omnisharp
```

# 93

edit ~/.npmrc

```shell
coc.nvim:registry=https://registry.npmmirror.com
```

# 92

add edge source

```shell
echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft-edge.gpg] https://packages.microsoft.com/repos/edge stable main' | sudo tee /etc/apt/sources.list.d/microsoft-edge.list

curl -fSsL https://packages.microsoft.com/keys/microsoft.asc | sudo gpg --dearmor | sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null
```

# 91

add unityhub source

```shell
sudo sh -c 'echo "deb https://hub.unity3d.com/linux/repos/deb stable main" > /etc/apt/sources.list.d/unityhub.list'

wget -qO - https://hub.unity3d.com/linux/keys/public | sudo tee /etc/apt/trusted.gpg.d/unityhub.asc
```

# 90

ubuntu chinese dirs to english dirs

```shell
export LANG=en_US
xdg-user-dirs-gtk-update

###
export LANG=zh_CN
或者
export LANG=zh_CN.utf-8
xdg-user-dirs-gtk-update
```

# 89

git 初始化远端仓库

```shell
git init
git add *
git branch -M main
git remote add origin git@xxx.git
git push -u origin main # 设置本地origin的远端上游main
```

# 88

包含

```shell
dpkg -i --force-overwrite xxx.deb
```

# 87

[asperite build](https://github.com/aseprite/aseprite/blob/main/INSTALL.md#linux-details)

# 86

relate local and remote branch

```shell
git branch -b main
git branch -u origin/remote_branch [local_branch]
git branch -vv
git pull rebase
# or
git branch --set-upstream-to orgin/remote_branch
```

# 85

git lfs

```shell
sudo apt install git-lfs
git lfs install
find ./ -size +100M 
git lfs track "*.suffix"
git add .gitattributes
git add all
git commit -m ""
git push -u orgin main
```

# 84

update ubuntu version

```
sudo apt install update-manager-core
sudo apt update && sudo apt dist-upgrade
sudo do-release-upgrade -d
```

# 83

safe-rm

```
/etc/safe-rm.conf
```

# 82

ssh: connect to host github.com port 22: Operation timed out

```
vi ~/.ssh/config
```

```
Host github.com
    Hostname ssh.github.com
    Port 443
```

```
ssh -T git@github.com
```

# 81

aseprite.desktop shortcut

```
$HOME/.local/share/applications/aseprite.desktop

[Desktop Entry]
Name=Aseprite
Exec=/opt/aseprite/aseprite
Terminal=false
Icon=/opt/aseprite/data/icons/ase.ico
Type=Application
```

# 80

split

```
split a.zip a.zip -d -b 1G
```

# 79

[xterm 256 colors](https://github.com/guns/xterm-color-table.vim)

# 78

youcompleteme c#

```
dotnet new sln # for ysm detection
```

# 77

disable snap

```
sudo systemctl disable snapd.service
sudo systemctl disable snapd.socket
sudo systemctl disable snapd.seeed.service
```

# 76

nvidia-dirver not found

```
sudo vim /etc/modprobe.d/blacklist.conf
# blacklist nouveau
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo ubuntu-drivers devices
```

# 75

close the ubuntu update-notifier

```
sudo chmod a-x /usr/bin/update-notifier
```

# 74

swap ecsape and cpas

```
sudo apt-get install dconf-editor
# /org/gnome/desktop/input-sources
# xkb-optims
# ['caps:swapescape']
```

# 73

dircolors

```
dircolors -p > .dir_colors
```

# 72

[zotero](https://www.zotero.org/download/)

```
tar -xzvf Zotero-version_linux-x86_64.tar.bz2
sudo mv zotero /opt/zotero
sudo /opt/zotero/set_launcher_icon
ln -s /opt/zotero/zotero.desktop ~/.local/share/applications/zotero.desktop
```

# 71

nvidia-smi to cool down

```
-lgc  --lock-gpu-clocks=    将指定的一对 <minGpuClock,maxGpuClock> 时钟
       频率取值范围（例如 1500,1500），设置为需锁定
       的 GPU 时钟频率的范围（以 MHz 为单位）。 
       无论 GPU 上是否存在正在运行的应用程序，设置此项
       将取代applications-clocks并生效。
       输入也可以是一个单一的期望时钟值
       （例如 <GpuClockValue>）。
-rgc  --reset-gpu-clocks    重置 GPU 时钟频率到默认值
```

# 70

aptitude

```
# 升级系统所有的软件包
aptitude update
# 将系统升级得到新的发行版
aptitude dist-uprade
# 安全升级系统的软件包
aptitude safe-upgrade
# 安装软件包
aptitude install pkgname
# 删除软件包保留配置 
aptitude remove pkgname 
# 删除软件包和配置 
aptitude purge pkgname
# 根据关键词搜索软件包
aptitude search string
# 查看软件包的详细信息
aptitude show pkgname
# 删除缓存目录中的软件包安装文件
aptitude clean
# 仅删除以卸载的软件包有关安装文件
aptitude autoclean
```

# 69

aria2c
errorCode=1 SSL/TLS handshake failure

```
aria2c url --check-certificate=false
```

# 68

update the remote list

```
git branch -r
git remote update origin --prune
```

# 67

wsl proxy

```
export all_proxy="socks5://allowlan:port"
```
<!--more-->

# 66

texlive

```
wget https://mirrors.huaweicloud.com/CTAN/systems/texlive/tlnet/install-tl-unx.tar.gz
tar -xzf install-tl-unx.tar.gz
cd install-tl-*
sudo ./install-tl -repository https://mirrors.huaweicloud.com/CTAN/systems/texlive/tlnet/
# /usr/local/texlive/...
tex --version
```

# 65

apt-fast

```
sudo add-apt-repository ppa:apt-fast/stable
sudo apt-get update
sudo apt-get -y install apt-fast
```

# 64

md5 check

```
md5sum -c md5file.txt
```

# 63

unzip multiple zip files

```
unzip '*.zip'
```

# 62

graphviz

```
sudo apt install graphviz
echo "digraph G {Hello->World}" | dot -Tpng > hello.png
vim hello.gv.txt 
# digraph G {Hello->World}
```

# 61

clone a directory from a repository using svn

```
# raw url https://github.com/google-research/google-research/tree/master/tiny_video_nets 
# replace tree/master with trunk
sudo apt-get install subversion
svn checkout https://github.com/google-research/google-research/trunk/tiny_video_nets
```

# 60

ubuntu remove xxx.deb(dpkg -i)

```
sudo dpkg -l
apt purge xxx
# apt remove 会删除软件包而保留软件的配置文件
# apt purge 会同时清除软件包和软件的配置文件
```

# 59

clash

```
# wget from github
# download your config.yaml and Country.mmdb into ~/.config/clash
gzip clash.gz
chmod +x clash
./clash
```

# 58

apt del PUBKEY

```
sudo apt-key del "xxxx  ... xxxx"
sudo apt-key del <last 8 characters>
```

# 57

apt NO\_PUBKEY

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F60F4B3D7FA2AF80
```

# 56

disable updates

```
# set 0
sudo vim /etc/apt/apt.conf.d/10periodic
sudo vim /etc/apt/apt.conf.d/20auto-upgrades
```

# 55

login the container

```
docker run -it --privileged=true ubuntu /sbin/init  
```

# 54

docker commit log

```
docker history image
```

# 53

journalctl

```
journalctl -u jupyterhub.service
```

# 52

grep -e and grep -E

```
-e<范本样式> --regexp=<范本样式>   # 指定字符串作为查找文件内容的范本样式。
-E --extended-regexp             # 将范本样式为延伸的普通表示法来使用，意味着使用能使用扩展正则表达式。
```

# 51

watch

```
watch -n 1 -d netstat -ant
```

# 50

ps table header

```
man ps
/STANDARD FORMAT SPECIFIERS
ps -eo pid,user,stat,command
-e every
-f full format
-o format/output
```

# 49

ssh -fNL 8889:localhost:8888 remote-server-alias

```
-f: fork background
-N: not execute remote command
-L: bind local:romote
```

# 48

github mirror

```
raw.githubusercontent.com -> raw.fastgit.org
github.com -> hub.fastgit.org
```

# 47

cuda and cudnn

```
1. download and install the cuda.
2. download and decompress cudnn. 
3. copy the include and lib into your cuda include and lib.
```

# 46

ccmake

```
sudo apt-get install cmake-curses-gui
# generate then confirm
```

# 45

show the information of graphics

```
lshw -c video
```

# 44

nporc

```
# Print the number of processing units available to the current process,which may be less than the number of online processors.
nporc
```

# 43

xrandr

```
xrandr
xrandr --output screen0 --auto
xrandr --output screen0 --mode1920x1080
xrandr --output screen0 --off
xrandr --output HDMI2 --same-as --auto
xrandr --output HDMI2 --primary
```

# 42

xserver VcXsrv

```
sudo apt install x11-apps -y && xeyes
export display=ip:port1.port2
export DISPLAY=:0.0
```

# 41

lsblk

```
get the information of disk
name, uuid, size, type, mount
```

# 40

testdisk

```
Partition scanner and disk recovery tool, and PhotoRec file recovery tool
```

# 39

install the package for debain

```
dpkg -l
dpkg -i package.deb
dpkg -r package 
```

# 38

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
sudo apt install docker-ce
docker pull image_url
docker run -itd \
 -v localhost:container 
 --name container_name
 --net=net_name
 --privileged=true
 --restart=always
 image_name
docker container ls
docker container stop/kill/restart/attach contain_id
docker image ls
docker rmi image_name
```

# 37

xargs -n 1

```
ls | xargs -n 1 echo
# dir1 
# dir2
ls | xargs echo
# dir1 dir2
```

# 36

update nodejs

```
npm install n -g
n stable
```

# 35

zsh-autosuggestions

```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# plugins=(zsh-autosuggestions) 
```

# 34

jupyterhub.service

```
[Unit]
Description=Jupyterhub

[Service]
User=root
#WorkingDirectory=/home/mlserver/
ExecStart=jupyterhub -f /home/mlserver/.jupyterhub/jupyterhub_config.py
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

# 33

change mode

```
chmod ugoa+-rwx file
u: user
g: group
o: other
r: read
w: write
x: execute
```
<!--more-->

# 32

list installed unit files

```
sudo systemctl list-unit-files
```

# 31

display the tree of processes

```
pstree 
```

# 30

linux boot process

1. BIOS(Basic Input/Output System)
2. MBR(Master Boot Record)
3. GRUB(Grand Unified Bootloader)
4. Kernel
5. Init
6. Runlevel

# 29

add/del a software source

```
add-apt-repository <sourceline>
apt-key list
apt-key del uid
```

# 28

top

```
VIRT - everything in-use and/or reserved
RES  - anything occupy physical memory 
SHR  - subset of RES
```

# 27

write a service

1. vim /etc/systemd/system/jupyter.service

```
[Unit]
Desciption=jupyter
[Service]
User=mlserver
WorkingDirectory=/home/mlserver/
ExecStart=jupyter-notebook
Restart=always
RestartSec=10
[Install]
WantedBy=muti-user.target
```

2. add service to system

```
# reload service
sudo systemctl daemon-reload
# start service
sudo systemctl start jupyter.service
# show service
sudo systemctl status jupyter.service
# autostart at boot 
sudo systemctl enable jupyter.service
```

# 26

nohup

```
# run the jupyter-notebook background and redirect the error
nohup jupyter-notebook > jupyter.log 2>&1 &
```
<!--more-->

# 25

mount -a
mount the filesystem in the /etc/fstab

# 24

use /etc/fstab to auto mount your disk

```
fdisk -l # find which dick you want to mount
vim /etc/fstab
# add line 
/dev/sda path/to/mount ext4 default 0 0
# 0(dump): Enable the dump command to backup filesystem
# 0(pass): Enable verifying the sector during startup
```

# 23

service

```
service --statuse-all
service [service name] start
service [service name] stop 
service [service name] status 
```

# 22

ssh server

```
sudo apt install openssh-server openssh-client
vim /etc/ssh/ssh_config
# find the line
PasswordAuthentication yes
service sshd restart
# set the ssh service
sudo systemctl enable ssh 
```

# 21

ubuntu install pip

```
sudo add-apt-repository universe
sudo apt install python3-pip
```

# 20

ubuntu system info

```
uname  -r && cat /etc/*release
```

# 19

add linux user

```
useradd -m <username>
passwd <username>
vim /etc/sudoers
```

# 18

awk [选项参数] 'script' var=value file(s)

```
# 每行按空格或TAB分割，输出文本中的1、4项
awk '{print $1,$4}' log.txt
```

# 17

ssh

```
# generate the ssh-key
ssh-keygen -t rsa -C "your_email@example.com"
# test ssh
ssh -Tv git@github.com
```

# 16

```
# git 查看remote
git remote -v
# git 修改 remote
git remote set-url origin xxx.git
```

# 15

git 统计author代码数

```
git log --format='%an' | sort | uniq -c | sort -r
```
<!--more-->

# 14

同步一个fork

```
# 查看远程状态
git remote -v

# 添加被同步给fork远程的上游仓库
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git

# 同步fork 
git fetch upstream

# 合并
git checkout master
git merge upstream/master
git push origin master
```
<!--more-->

# 13

alias

```
alias hexocld='hexo cl && hexo d'
```

# 12

archlinux 安装大致思路

```
# 分区
fdisk
# 安装linux固件
pacstarp
# 配置引导
genfstab
grub
# 设置时间、时区、语言、网络
```

# 11

将脚本demo.sh的标准输出和标准错误输出重定向至文件demo.log

```
bash demo.sh &> demo.log
# 将标准输出(stdout)和标准错误输出(stderr)重定向至指定文件中
bash demo.sh > demo.log 2>&1
# command > file 2>&1，是由两部分组成。
# 1. command> file 表示将标准输出(stdout)重定向到文件file中。
# 2. 2>&1 表示将标准错误输出(stderr)重定向到文件描述符1指定位置。
```

# 10

/var/log/messages — 包括整体系统信息，其中也包含系统启动期间的日志。此外，mail，cron，daemon，kern和auth等内容也记录在var/log/messages日志中。

# 9

linux log

| log | doc |
| --- | --- |
| /var/log/lastlog | 记录系统中所有用户最后一次登录时间的日志。这个文件是二进制文件。可以使用lastlog命令查看。 |
| /var/log/wtmp | 永久记录所有用户的登录、注销信息，同时记录系统的启动、重启、关机事件。同样，这个文件也是二进制文件，不能使用vi查看，而要使用last 命令查看 |
| var/log/utmp | 记录当前已经登录的用户信息。这个文件会随着用户的登录和注销而不断变化，只记录当前登录用户的信息。同样，这个文件不能使用vi查看，而要使用w,who,users命令查看 |

# 8

bash 运算符

| 运算符 | 说明 | 举例 |
| ------ | ---- | ---- |
| ! | 非运算符，表达式为true返回false，否则返回true | [!false]返回true |
| -o | 或运算符，有一个表达式为true则返回true | [$a -lt -o $b -gt 100] 返回true |
| -a | 与运算符，两个表达式都为true则返回true | [$a -lt -a $b -gt 100] 返回false |

# 7

nohup(no hang up),用于系统后台不断地运行命令，退出终端不会影响程序运行。
在后台执行root目录下的runoob.sh

```
nohup /root/runoob.sh &
```

# 6

| 命令 | 查看方式 |
| ---- | -------- |
|cat | 由第一行开始显示文件所有内容 |
|tac | 从最后一行开始显示文件所有内容 |
|more | 一页一页的显示文件内容，只能向后翻页 |
|less | 也是一页一页的显示文件内容，但是可以通过键盘上的PageDown、PageUp控制向后向前翻页，可以光标移动|
|head | 显示一个文件的前几行 |
|tail | 显示一个文件的后几行 |

# 5

硬链接&软连接2.0
**什么是连接?**
链接操作实际上是给系统中已有的某个文件指定另外一个可以用于访问它的名称。对于这个新的文件名，我们可以为之指定不同的访问权限，可以控制对信息的共享和安全的问题。如果连接指向目录，用户就可以利用该链接直接进入链接的目录而不用打一大堆的路径名。而且即使我们删除这个链接也不会破坏原来的目录。
**硬连接**
硬链接只能引用同一文件系统中的文体。它引用的是文件在文件系统中的物理索引(inode)。当您移动或删除原始文件时，硬链接不会被破坏，因为它所用引用的是文件的物理数据而不是文件结构中的位置。硬链接的文件不需要用用户有访问原始文件的权限，也不会显示原始文件的位置，这样有助于文件的安全。如果您删除的文件有相应的硬链接，文件依然会被保留，直到所有对它的引用都被删除。
**软连接**
软连接，其实就是新建一个文件，这个文件就是专门用来指向别的文件(和windows下的快捷方式的那个文件有很近的意思)。软连接产生的是一个新文件，但这个文件的作用就是专门指向某个文件的，删除了这个软连接文件，那就等于不需要这个链接，和原来的存在实体原文件没有任何关系，但删除原来的文件，则相应的软连接不可用(cat那个软连接文件，则提示“没有该文件或目录”)。

# 4

linux 目录

| 目录       |    内容      |
| ------- |-------------|
| /bin  | Binaries,系统可执行文件目录|
| /boot | Boot,启动文件目录|
| /dev  | Device,外部设备目录|
| /etc  | Etcetera,系统配置文件目录|
| /home | Home, 用户主目录|
| /lib  | Libaray,系统的库文件|
| /media| Media,系统自动识别的一些设备，如U盘、光驱 |
| /mnt  | Mount,用户挂载临时文件目录 |
| /opt  | Optional, 第三方软件目录|
| /proc | Processes, 存储当前内核运行的一系列特殊文件,虚拟目录 |
| /root | Root, 系统管理员目录|
| /sbin | Superuser Binaries, 系统管理员可执行程序目录 |
| /srv  | Services, 存放服务启动后需要提取的数据 |
| /tmp  | Temporary， 临时文件目录|

# 3

压缩

```shell
tar -czvf output.zip input
```

解压

```shell
tar -xzvf input.zip  output
```

# 2

linux 启动过程

1. Power-up/Reset  
2. System startup  

- BIOS/BootMonitor/EFI

3. Stage 1 bootloader  

- Master Boot Record

4. Stage 2 bootloader

- LILO, GRUB, etc

5. Kernel & Linux
6. Init & User-space
7. Operation

# 1

查找当前目录一个月(30天)以前大于100M的日志文件(.log)并删除

```shell
find . -name "*.log" -mtime +30 -type f -size +100M | xargs rm -rf 

find 查找 
- . 在当前目录查找
- -mtime 指定修改时间以天为单位 
 +xx 修改时间大于xx 
 -xx 修改时间小于xx 
-type 指定文件类型 
-size 指定文件大小 
 +xx 修改时间大于xx 
 -xx 修改时间小于xx 
```
