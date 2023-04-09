# Linxu笔记

### vim编辑器

 1. **是什么？**

    > VI 是 Unix 操作系统和类 Unix 操作系统中最通用的文本编辑器。VIM 编辑器是从 VI 发展出来的一个性能更强大的文本编辑器。可以主动的以字体颜色辨别语法的正确性，方便程序设计。VIM 与 VI 编辑器完全兼容

2. **一般模式**

   > 以 vi 打开一个档案就直接进入一般模式了（这是默认的模式）。在这个模式中， 你可以使用『上下左右』按键来移动光标，你可以使用『删除字符』或『删除整行』来处理档案内容， 也可以使用『复制、粘贴』来处理你的文件数据

<img src="img/%E4%B8%80%E8%88%AC%E6%A8%A1%E5%BC%8F%E5%B8%B8%E7%94%A8%E8%AF%AD%E6%B3%95.png" style="zoom:40%;" />

3. **编辑模式**

   > 在一般模式中可以进行删除、复制、粘贴等的动作，但是却无法编辑文件内容的！要等到你按下『i, I, o, O, a, A』等任何一个字母之后才会进入编辑模式。 
   >
   > 注意了！通常在Linux中，按下这些按键时，在画面的左下方会出现『INSERT或REPLACE』的字样，此时才可以进行编辑。而如果要回到一般模式时， 则必须要按下『Esc』这个按键即可退出编辑模式。 

<img src="img/%E7%BC%96%E8%BE%91%E6%A8%A1%E5%BC%8F%E5%B8%B8%E7%94%A8%E8%AF%AD%E6%B3%95.png" style="zoom:50%;" />

4. **指令模式**

   >在一般模式当中，输入『 : / ?』3个中的任何一个按钮，就可以将光标移动到最底下那一行。在这个模式当中， 可以提供你『搜寻资料』的动作，而读取、存盘、大量取代字符、离开 vi 、显示行号等动作是在此模式中达成的！

<img src="img/%E6%8C%87%E4%BB%A4%E6%A8%A1%E5%BC%8F%E5%B8%B8%E7%94%A8%E8%AF%AD%E6%B3%95.png" style="zoom:46%;" />

5. **模式转换**

   <img src="img/%E6%A8%A1%E5%BC%8F%E5%88%87%E6%8D%A2.png" style="zoom:50%;" />

### 配置网络地址

1. **_ifconfig_配置网络接口**

   * 基本语法

     >ifconfig  			（功能描述：显示所有网络接口的配置信息）

   * 实例实操

     > 查看当前网络 ip   
     >
     > $ ifconfig

2. **_ping_测试主机之间网络连通性** 

   * 基本语法

     > ping 目的主机 				（功能描述：测试当前服务器是否可以连接目的主机）

   * 实例实操

     > $ping www.baidu.com

3. **修改IP地址**

   * **查看IP配置文件** 

     > vim /etc/sysconfig/network-scripts/ifcfg-ensxx    				(enxx  使用 ifconfig 查看)

   * **修改配置文件**

     <pre>
     TYPE="Ethernet" #网络类型（通常是 Ethemet） 
     PROXY_METHOD="none" 
     BROWSER_ONLY="no" 
     <font color="red">BOOTPROTO="static"</font>  #IP 的配置方法[none|static|bootp|dhcp]（引导时不使用协议|静态分配 IP|BOOTP 协议|DHCP 协议）
     DEFROUTE="yes" 
     IPV4_FAILURE_FATAL="no" 
     IPV6INIT="yes" IPV6_AUTOCONF="yes" 
     IPV6_DEFROUTE="yes" 
     IPV6_FAILURE_FATAL="no" 
     IPV6_ADDR_GEN_MODE="stable-privacy" 
     NAME="ens33" 
     UUID="e83804c1-3257-4584-81bb-660665ac22f6" #随机 id 
     DEVICE="ens33" #接口名（设备,网卡） 
     <font color="red">ONBOOT="yes"</font> #系统启动的时候网络接口是否有效（yes/no） 
     <font color="red">#IP 地址 
     IPADDR=192.168.1.100 
     #网关 
     GATEWAY=192.168.1.2 
     #域名解析器 
     DNS1=192.168.1.2</font>
     </pre>

   * **执行_Systemctl restart network_重启网络**

4. **修改IP地址后可能会遇到的问题** 

   > (1)	物理机能 ping 通虚拟机，但是虚拟机 ping 不通物理机,一般都是因为物理机的防火墙问题,把防火墙关闭就行
   >
   > (2)	虚拟机能 Ping 通物理机,但是虚拟机 Ping 不通外网,一般都是因为 DNS 的设置有问题
   >
   > (3)	虚拟机 Ping www.baidu.com 显示域名未知等信息,一般查看 GATEWAY 和 DNS 设 置是否正确 
   >
   > (4)	如果以上全部设置完还是不行，需要关闭 NetworkManager 服务 
   >
   > ​		> systemctl stop NetworkManager 关闭
   >
   > ​		> systemctl disable NetworkManager 禁用 
   >
   > (5)	如果检查发现 systemctl status network 有问题 需要检查 ifcfg-ensxxw

### 配置主机名称

1. **修改主机名称**

   * **基本用法**

     > hostname 					（功能描述：查看当前服务器的主机名称） 

   * **案例实操** 

     > （1）查看当前服务器主机名称 
     >
     > [root@hadoop100 桌面]# hostname 
     >
     > （2）如果感觉此主机名不合适，我们可以进行修改。通过编辑/etc/hostname 文件 
     >
     > [root@hadoop100 桌面]# vi /etc/hostname 
     >
     > 修改完成后重启生效。 

2. **修改** **hosts** **映射文件**

   * **修改 linux 的主机映射文件（hosts 文件）**

     > 后续在 hadoop 阶段，虚拟机会比较多，配置时通常会采用主机名的方式配置，比较简单方便。 不用刻意记 ip 地址。
     >
     >  
     >
     > （1）打开/etc/hosts 
     >
     > [root@hadoop100 桌面]# vim /etc/hosts 
     >
     > ​			添加如下内容 
     >
     > 192.168.2.100 hadoop100 
     >
     > （2）重启设备，重启后，查看主机名，已经修改成功

   * **修改 mac 的主机映射文件（hosts 文件）**

     > （1）进入 /etc 路径 
     >
     > （2）拷贝 hosts 文件到桌面 (cp /etc/hosts /Users/qingyuan/Desktop/hosts)
     >
     > （3）打开桌面 hosts 文件并添加如下内容 
     >
     > ​			192.168.2.100 hadoop100
     >
     > （4）将桌面 hosts 文件覆盖 \etc 路径 hosts 文件

### 远程登录

> 通常在工作过程中，公司中使用的真实服务器或者是云服务器，都不允许除运维人员之外的员工直接接触，因此就需要通过远程登录的方式来操作。所以，远程登录工具就是必不可缺的，目前，比较主流的有 Xshell, SSH Secure Shell, SecureCRT,FinalShell 等，同学们可以根据自己的习惯自行选择. 

### 常用基本命令

​	见[40个常用基本命令](https://www.wbolt.com/most-used-linux-commands.html#linux-commands-cheat-sheet)