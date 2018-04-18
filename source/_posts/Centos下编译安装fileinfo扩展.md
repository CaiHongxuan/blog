---
title: Centos下编译安装fileinfo扩展
date: 2017-09-13 17:50:07
tags:
  - centos
  - php
  - fileinfo
categories:
  - Other
---

这两天发现在服务器上(linux系统)没有安装fileinfo扩展，导致上传文件等操作失败。

于是，尝试使用如下命令安装该扩展：
```
pecl install fileinfo
```
结果失败。说什么.m4文件不存在等问题。

最后。上网查了下相关资料，通过如下方式才得以成功：

### 1、检查当前环境：
```
php -i|grep fileinfo
```
看是否已安装fileinfo扩展，若没有，则进行下一步。

### 2、安装fileinfo扩展

#### 2.1、下载扩展包
**根据各自的版本号进行下载**
```
wget -O php-5.6.25.tar.gz http://cn2.php.net/get/php-5.6.25.tar.gz/from/this/mirror
```

#### 2.2、解压
```
tar -zxvf php-5.6.25.tar.gz
```

#### 2.3、进入该扩展目录
```
cd /alidata/server/php/php-5.6.25/ext/fileinfo
```
该扩展暂时解压在`/alidata/server/php`目录下

#### 2.4、编译 && 安装
```
/alidata/server/php/bin/phpize
./configure -with-php-config=/alidata/server/php/bin/php-config
make && make install
```
这样，就会在系统默认的扩展目录下新生成一个`fileinfo.so`文件

#### 2.5、修改php.ini文件
加入：`extension=fileinfo.so`

完成。

参考文章：[Centos 下PHP编译安装fileinfo扩展](https://segmentfault.com/a/1190000005058875)
