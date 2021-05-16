# hadoop-cluster-docker-centos
## 使用
现在集群配置了一主四从，可以修改slaves文件减少从节点数量
## 问题
### 问题1
```$’\r’: command not found:```
#### 问题原因
这是因为Windows系统的文件换行使用的是\r\n，而Unix系统是\n
#### 解决办法
```
安装dos2unix来进行文件转换
yum install -y dos2unix
dos2unix aaa.sh
```
OR
```
使用vim打开文件，然后使用命令:set ff=unix，保存文件
# 使用vim打开文件
vim aaa.sh
# 转换格式
:set ff=unix
# 保存文件
:wq
```
### 问题2
Name or service not knownstname hadoop-slave1
#### 问题原因
slaves文件可能被污染了。
#### 解决办法
删除掉slaves文件，重新建立一个slaves文件，并配置好就可以了。
### 问题3
System has not been booted with systemd as init system
#### 问题原因
Linux Docker中无法使用 systemd(systemctl) 相关命令的原因是 1号进程不是 init ，而是其他例如 /bin/bash ，所以导致缺少相关文件无法运行。（System has not been booted with systemd as init system (PID 1). Can't operat）。
#### 解决办法
docker run -tid --name test --privileged=true ubuntu:18.04 /sbin/init
--privilaged=true一定要加上的
### 因为tuna上的镜像更新后导致构建失败，那么需要将hadoop和java的下载地址修改成现版本地址