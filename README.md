Node Ping Manage
---------------------

描述
------------
本项目通过ping的方式监测设备的死活，出现状态变化即触发报警， 报警方式可以是邮件／短信。


启动方法
----------------
```bash
python npm.py
```

推荐添加计划任务
---------------
```bash
 */3 * * * * python /root/npm/npm.py  > /dev/null
```

配置文件格式
-----------------------
| 设备IP | 设备描述|初始状态关键字|邮箱|手机号|
|-----|------|----|----|----|
|114.114.114.114|CoreSwExample|Init|yihf@liepin.com|13521161889
|192.168.1.1|Server1|Init|yihf@liepin.com|13521161889


邮件短信报警模块配置
---------------------------
```bash
git clone  https://github.com/hongfeioo/messagemodule.git
```
[messagemodule](https://github.com/hongfeioo/messagemodule)</p>



Up,Down 报警状态机
--------------
>  5次ping测试中，有一次通则为：Up </p>
>  5次ping测试中，全部超时则为：Down , 每次ping超时时间为2秒</p>
>  检测次数和超时时间在ping.py中verbose_ping函数参数中修改 </p>



小技巧
-----------
1   主程序中  sms_off 默认为0 ，如果为1则全局关闭短信。  </p>
2   主程序中  mail_off 默认为 0 ，如果为1则全局关闭邮件发送。  </p>
3   主程序中  MAX_process 默认为300， 用于限制ping的并发数。  </p>
4   手机号或者邮箱中如果出现null字符串则跳过这个联系人. </p>
5   当接受报警的是多邮箱或者多手机号时用分号隔开即可. </p>
6   当你要部署多套npm的时候，npm_title 变量用来区分报警是从哪个节点产生的。</p>

程序原理介绍
---------
1  主程序第一次运行时，从配置文件npm.ini中读取每行信息，并发对每一行的主机ip进行ping测试, 探测的结果会写入npm.tmp文件.</p>
2  当程序第二次运行时，会先读取npm.tmp文件中数据作为参考，然后进行第二次ping探测，如果发现本次探测结果和参考状态不符，则说明状态有变化，触发报警,并把最新状态存入npm.tmp。</p>
3  如果npm.ini进行了调整，需要删除npm.tmp文件。

排错 
------
1   配置文件末尾请不要留空行.</p>
2   所有日志默认输出的位置是：/root/mylog.txt  </p>
3   如果修改了pingModule中的文件，需要删除*.pyc  </p>

开发环境
--------
Python 2.7.5 

作者介绍
----------
yihongfei  QQ:413999317   MAIL:yihf@liepin.com

CCIE 38649


寄语
------
麻雀虽小五脏俱全，为网络自动化运维尽绵薄之力 </p>



TODO
----------
#NCM： 自动备份交换机、路由器的配置
#UDT： 自动抓取交换机的mac表和arp表并进行匹配，通过web页面清晰查询IP-MAC-PORT之间的关系。
#alllogscan：自动检测log文件中的关键字，发送邮件短信报警。例如：交换机、路由器、防火墙、服务器的log不限。
#thresholdWarning：自动登录交换机、路由器、防火墙，抓取cpu，端口数值，并实现邮件短信报警。
#ChangeVlanbyself：实现自助划分vlan的功能，让非网络人员也可以划分交换机端口，操作简单，内涵4层安全检查，杜绝误操作引起的网络故障。



