# ikoa-vps   beta
[![BSD-3](https://img.shields.io/badge/LICENSE-BSD3-brightgreen.svg)](https://github.com/horryruo/multi-bot/blob/master/LICENSE)

**由项目ikoa-web衍生而来，用于部署于vps上以供下载。**  

## 免责声明：本代码仅用于学习，下载后请勿用于商业用途，本人对此有最终解释权。
## Disclaimer: This code is only for learning, please do not use it for commercial purposes after downloading, I have the final right of interpretation.
## Tips
1、由于本人只略懂python，其他的docker、shell没接触过，临时学习，只能大致更改到可以使用的程度（有小bug也不奇怪）

2、本项目只经过少量测试，且本人也是菜鸟，有bug在所难免。出现bug如果有能力的可以自己修修，可以直接提issue并把问题怎么解决贴出来。而只提无意义问题的概不回复。大bug且我会修的话会尽快修复。。


## install
  由于本人能力有限，不能实现完全自动化， 部分文件任需手动，
  以下过程需要一定的linux基础， 不懂的自行谷歌(⊙o⊙)
  
1、安装 Python 3.6+  版本

2、`git clone https://github.com/waahaa/ikoa-vps.git && chmod +x ikoa-vps` 

3、`cd ikoa-vps`  

4、`pip3 install -r requirements.txt`  

5、`cp config.ini.example config.ini` 

6、安装rclone    `curl https://rclone.org/install.sh | sudo bash`

7、 确保你的ikoa可以使用，本程序默认使用64位ikoa，如要使用32位，把fanza文件夹"iKOA"重命名，然后把"iKOA_32位"重命名为"iKOA"(区分大小写),且先安装ikoa所需依赖（32位glibc 2.15，64位glibc 2.28，请自行谷歌如何安装），然后确认iKOA权限为755权限，否则无法启动。检查 `./iKOA` 可以单独启动iKOA才进行下一步。

   参考(系统多种多样，不一定通用，不能启动时还是谷歌吧)：debian:`sudo apt-get install libc6:i386 lib32z1`centos:`yum install -y glibc glibc-common glibc-devel glibc-headers glibc-static glibc-utils`

8、原程序带有unix软件包，先自行安装：centos: `yum -y install epel-release && yum -y install moreutils`     debian:`apt-get install moreutils`

9、配置config.ini,内有说明（ikoa自带的config.toml不需自己配置，但是也可以根据自己需要更改某些配置，但是更改后本程序不一定能成功执行，例如ikoa多开，本程序并没有做多进程多开功能，因此开启了也是不能多个同时下载的）

10、原sa文件，请自行在fanza文件夹创建两个文件service_account_1.json、service_account_2.json，并分别把sa全部内容复制进去即可。（sa文件不会参与git同步）

11、确保配置完毕，即可在ikoa-vps目录下，创建screen（后台运行，具体谷歌），然后运行`python3 start.py`,接着就可以在浏览器输入你的vps ip：端口，看到熟悉的页面

12、可使用nginx/caddy反代0.0.0.0:端口 即可实现https访问

13、<del>如若配置失败，或者运行失败需要重新运行，往往会被占用端口，使用命令lsof -i:端口可查看所有占用端口程序，再使用`kill -9 pid`结束所有进程再开启服务。`kill -9 $(lsof -i:端口|awk '{if(NR==2)print $2}')` </del>
目前已支持自动关闭占用端口程序

### update
2.1  --- 2020/11/21   增加gd单账号支持，本地配置好后，除标题外复制到rclone_4.conf，保留'[DRIVE]'标题，此时的团队盘id使用的是你配置的而不是config.ini的。

2.0  --- 2020/10/28   支持实时显示日志,但添加任务后需等待任务完成才能继续添加。

### 上传到onedrive
目前为测试版，请在本地rclone配置好onedrive配置文件（自行谷歌），然后在fanza文件夹copy一份`cp rclone_3.conf.temp rclone_3.conf` ，然后把本地配置好的除名字外全部内容复制到rclone_3.conf,注意不要删掉原来的[DRIVE]标题，此为程序内置名字，更改后无法上传。
###  Bug
第一次启动可能会报一次错，请多次启动

debian以及ubuntu系统报错请先更新系统以及软件包`sudo apt update && sudo apt upgrade`

<del>输入内容确认后，可能内容不会自动清空，原因未明（我不懂），只要没报错都是添加到任务了。</del>已解决，目前只能添加一次任务，上一次结束后才能继续添加。

<del>由于某个命令运行出错，暂时无法在web界面显示日志，后续想办法解决</del>已解决


