# 树莓派使用记录

## 树莓派系统

+ [Debian-Pi-Aarch64](https://gitee.com/openfans-community/Debian-Pi-Aarch64/blob/master/README_zh.md)
+ [点击前往百度网盘下载](https://pan.baidu.com/share/init?surl=VPWngCO1aEPJXFMLiODmNg) 提取码: xbwy

## 校验包，解压

```shell
sha1sum 2013-09-25-wheezy-raspbian.zip
unzip  2013-09-25-wheezy-raspbian.zip
```

查看当前哪些设备已经挂载,`df -h`，插入u盘或sd卡再执行一次
为了防止在写入镜像的时候有其他读取或写入，我们需要卸载设备。两个分区都要卸载。

```shell
umount  /dev/sdb1
umount  /dev/sdb2
```

## xz烧录命令(该命令尝试失败)

```shell
sudo xz -cd kali-2017.3-rpi3-nexmon.img.xz> /dev/sdb
```

查看烧录进度`sudo pkill -USR1 -n -x xz`

## img格式镜像烧录命令如下（亲测成功）

```shell
sudo dd bs=4M if=2013-09-25-wheezy-raspbian.img of=/dev/sdb
```

查看烧录进度`sudo pkill -USR1 -n -x dd`

## 卸载软件

卸载但不删除配置

```shell
apt-get remove packagename
```

卸载并删除配置

```shell
apt-get purge packagename
```

## 基础命令

[原链接](https://shumeipai.nxez.com/2015/01/03/raspberry-pi-software-installation-and-uninstallation-command.html)

安装软件 `apt-get install softname1 softname2 softname3……`

卸载软件 `apt-get remove softname1 softname2 softname3……`

卸载并清除配置 `apt-get remove –purge softname1`

更新软件信息数据库 `apt-get update`

进行系统升级 `apt-get upgrade`

搜索软件包 `apt-cache search softname1 softname2 softname3……`

安装deb软件包 `dpkg -i xxx.deb`

删除软件包 `dpkg -r xxx.deb`

连同配置文件一起删除 `dpkg -r –purge xxx.deb`

查看软件包信息 `dpkg -info xxx.deb`

查看文件拷贝详情 `dpkg -L xxx.deb`

查看系统中已安装软件包信息 `dpkg -l`

重新配置软件包 `dpkg-reconfigure xxx`

清除所有已删除包的残馀配置文件

```shell
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
```

> dpkg安裝的可以用apt卸載，反之亦可。

## aptitude

`aptitude update` 更新可用的包列表

`aptitude upgrade` 升级可用的包

`aptitude dist-upgrade` 将系统升级到新的发行版

`aptitude install pkgname` 安装包

`aptitude remove pkgname` 删除包

`aptitude purge pkgname` 删除包及其配置文件

`aptitude search string` 搜索包

`aptitude show pkgname` 显示包的详细信息

`aptitude clean` 删除下载的包文件

`aptitude autoclean` 仅删除过期的包文件

## 安装owncloud

教程开始：

1、打开树莓派终端，输入下面命令获得超级用户权限

```shell
sudo -i
```

2、在docker中安装私有云，在树莓派终端输入下面命令。（注意：为了避免出错，下面用到的所有命令以及要修改的内容也给大家提供了文档版本，可以在树莓派爱好者基地微信公众号发送【私有云安装】获得）

```shell
docker run -d -p 8888:80  --name nextcloud  -v /data/nextcloud/:/var/www/html/ --restart=always   --privileged=true  arm64v8/nextcloud
```

3、修改配置文件

```shell
nano /data/nextcloud/config/config.sample.php
```

修改`trusted_domains`里的值

```txt
1 => preg_match('/cli/i',php_sapi_name())?'127.0.0.1':$_SERVER['SERVER_NAME']
```

4、安装nextcloud客户端

```shell
apt install nextcloud-desktop-l10n nextcloud-desktop -y
```
