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

## 时间的输出和设置时间
输出时间
```
date +"%Y-%m-%d %H:%M:%S.%N  %u"
#  输出为 2019-09-07 01:59:48.527513056  6
#  表示当前时间为2019年9月7日1时59分48秒 527513056纳秒 6表示周六

date +"%p"
#  输出当前时AM还是PM

date +"%s"
#  输出自 1970-01-01 00:00:00 UTC 的秒数
```


## 时间的概念
GMT Time是指 Greenwich Mean Time，是指0度经线地区的时间，因为0度经线经过格林威治，
因此以格林威治地区的时间作为时间统一的标准，称为GMT Time。

对于不同的地区来说，当地由于与0度经线有差别，当地时间需要在GMT Time的基础上加上或减去
一些时间。(以我浅薄的知识认为，以太阳来定义时间，太阳直射的经线，该经线坐在地区即为中午12点。
因为太阳直射经线会随着地球自转而变化，因此就导致了不同地区的中午12点有先有后，这就导致了不同地区的当地时间所有不同)

UTC时间是指Universal Time Coordinated，是时间统一的标准。在1972年之前称为GMT Time。因此UTC和GMT时间是相同的时间。

对于程序员来说，时间的保存通常用UTC时间来保存，这样就不需要考虑不同地区时间的差异。当需要把时间取出来进行展示时，根据应用程序或主机对地区的设置，将UTC时间转化为当地时间(localtime)进行展示。


