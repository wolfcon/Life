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
