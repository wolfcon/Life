<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [crontab 定时任务的介绍](#crontab-%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1%E7%9A%84%E4%BB%8B%E7%BB%8D)
  - [简介](#%E7%AE%80%E4%BB%8B)
  - [cron 一共有3个配置](#cron-%E4%B8%80%E5%85%B1%E6%9C%893%E4%B8%AA%E9%85%8D%E7%BD%AE)
    - [1. /var/spool/cron/](#1-varspoolcron)
    - [2. /etc/crontab](#2-etccrontab)
    - [3. /etc/cron.d/](#3-etccrond)
  - [cron 的日志](#cron-%E7%9A%84%E6%97%A5%E5%BF%97)
  - [简单的例子](#%E7%AE%80%E5%8D%95%E7%9A%84%E4%BE%8B%E5%AD%90)
    - [1,2,3,59 * * * * root echo "hello, cron!"](#12359-----root-echo-hello-cron)
    - [除了数字还有几个特殊的符号就是 `*`, `/`, `-`, `,`，`*` 代表所有的取值范围内的数字，`/` 代表每的意思, `/5` 表示每5个单位，`-` 代表从某个数字到某个数字, `,` 分开几个离散的数字](#%E9%99%A4%E4%BA%86%E6%95%B0%E5%AD%97%E8%BF%98%E6%9C%89%E5%87%A0%E4%B8%AA%E7%89%B9%E6%AE%8A%E7%9A%84%E7%AC%A6%E5%8F%B7%E5%B0%B1%E6%98%AF------%E4%BB%A3%E8%A1%A8%E6%89%80%E6%9C%89%E7%9A%84%E5%8F%96%E5%80%BC%E8%8C%83%E5%9B%B4%E5%86%85%E7%9A%84%E6%95%B0%E5%AD%97-%E4%BB%A3%E8%A1%A8%E6%AF%8F%E7%9A%84%E6%84%8F%E6%80%9D-5-%E8%A1%A8%E7%A4%BA%E6%AF%8F5%E4%B8%AA%E5%8D%95%E4%BD%8D--%E4%BB%A3%E8%A1%A8%E4%BB%8E%E6%9F%90%E4%B8%AA%E6%95%B0%E5%AD%97%E5%88%B0%E6%9F%90%E4%B8%AA%E6%95%B0%E5%AD%97--%E5%88%86%E5%BC%80%E5%87%A0%E4%B8%AA%E7%A6%BB%E6%95%A3%E7%9A%84%E6%95%B0%E5%AD%97)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[TOC]

# crontab 定时任务的介绍

## 简介
下面这个是官方 crontab 的配置文件介绍
```
For details see man 4 crontabs
Example of job definition:
.---------------- minute (0 - 59)
| .------------- hour (0 - 23)
| | .---------- day of month (1 - 31)
| | | .------- month (1 - 12) OR jan,feb,mar,apr ...
| | | | .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
| | | | |
* * * * * user-name command to be executed
```

## cron 一共有3个配置
### 1. /var/spool/cron/ 
这个`目录`下存放的是每个用户包括 `root` 的 `crontab` 任务, 每个任务以创建者的名字命名, 比如 `root` 建的 crontab 任务对应的文件就是 `/var/spool/cron/root`. 一个用户最多只有一个 crontab 文件.

这个文件不可编辑, 是由 `crontab` 命令创建的, 具体命令参照 `info crontab` 查看, 其中还涉及到权限等.

### 2. /etc/crontab 
这个`文件`负责安排由系统管理员制定的维护系统以及其他任务的`crontab`.

### 3. /etc/cron.d/
这个`目录`用来存放任何要执行的`crontab`文件或脚本.

## cron 的日志

日志的目录一般是在 /var/log/ 下.

## 简单的例子

### 1,2,3,59 * * * * root echo "hello, cron!"
这个就是在`每个小时`的`1分`, `2分`, `3分`以及`59分`时, 用 `root` 用户权限执行打印 `hellow, cron`

### 除了数字还有几个特殊的符号就是 `*`, `/`, `-`, `,`，`*` 代表所有的取值范围内的数字，`/` 代表每的意思, `/5` 表示每5个单位，`-` 代表从某个数字到某个数字, `,` 分开几个离散的数字
