# 关于linux中的日期和时间

## 如何让虚拟机中的Linux与本机的同步
虚拟机若是VMWare，选择设置->选项->VMware Tools->勾选**将客户机时间与主机同步**。
然后重新启动虚拟机，客户机的时间将自动同步为主机的时间。

若时间仍有差别，可能是时区的问题。使用如下命令，将/etc/localtime软链接到/usr/share/zoneinfo/Asia/Hong_Kong，就可以将时区设置为香港时区。或者直接将时区文件(tzfile)复制并重命名为/etc/localtime。
```
#方式一: 创建软链接,需要/etc/localtime不存在
ln -s /usr/share/zoneinfo/Asia/Hong_Kong /etc/localtime

#方式二: tzfile复制并重命名为/etc/localtime
cp /usr/share/zoneinfo/Asia/Hone_Kong /etc/localtime
```