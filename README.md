# vultr搭建ssr服务器，并使用ipv6免流上校园网教程（CentOs8避坑指南）

不怕被封ip，因为vultr是折算成小时计费，且可以随时删除和开通服务器，新服务器就是新的ip。新开服务器只需要0.01美元，即使你运气非常不好，开了10台服务器才获得没有被墙的ip，总创建服务器成本也只有0.1美元，不到1块钱。开通服务器时，当出现了ip，不要立马去ping或者用xshell去连接，再等5分钟之后，有个缓冲时间。

自建ss/ssr教程很简单，整个教程分三步：

第一步：购买VPS服务器

第二步：一键部署VPS服务器

第三步：一键加速VPS服务器 (centos6系统选择锐速加速，cenots7选择bbr加速)

# 第一步：购买VPS服务器

需要注意的一点是：我们一定要自己注册帐号， 千万不要找人代购 ，因为代购存在很大的风险，VPS可能被植入后门程序，十分危险，而且被主机商发现会被封号处理。 

## **1.注册Vultr账号**

打开 [ Vultr官网 ](https://www.vultr.com/?ref=8557194-6G) ，输入邮箱和密码，点击“Create Account” 即可。

## **2.账户激活**

新用户注册后就自动跳转到充值页面，如下图所示。可选择不同的充值方式，我们推荐使用支付宝，当然信用卡、Paypal、比特币也是OK的。

注意：Vultr为避免帐号滥用，帐号在使用前需要进行激活，激活条件是： **最低充值10美元** 。10美元买$2.5可以用4个月，没有时间限制，条件不算苛刻。

点击Pay with Alipay后，跳转到支付宝的付款页面，又是熟悉的画面，自动换成人民币完成充值。

vultr在2020年的最新活动，针对新用户，直接送100美元（一个月有效）！全球15个服务器位置可选，kvm框架。如果以后这个vultr注册地址被墙了，那么就用翻墙软件打开，或者用[ss/ssr免费账号](https://github.com/Alvin9999/new-pac/wiki/ss免费账号)

##  3.创建VPS服务器实例

vultr实际上是折算成小时来计费的，比如服务器是5美元1个月，那么每小时收费为5/30/24=0.0069美元 会自动从账号中扣费，只要保证账号有钱即可。如果你部署的服务器实测后速度不理想，你可以把它删掉（destroy），重新换个地区的服务器来部署，方便且实用。因为新的服务器就是新的ip，所以当ip被墙时这个方法很有用。当ip被墙时，为了保证新开的服务器ip和原先的ip不一样，先开新服务器，开好后再删除旧服务器即可。

计费从你开通服务器开始算的，不管你有没有使用，即使服务器处于关机状态仍然会计费，如果你没有开通服务器就不算。比如你今天早上开通了服务器，但你有事情，晚上才部署，那么这段时间是会计费的。同理，如果你早上删掉服务器，第二天才开通新的服务器，那么这段时间是不会计费的。在账号的Billing选项里可以看到账户余额。

温馨提醒：同样的服务器位置，不同的宽带类型和地区所搭建的账号的翻墙速度会不同，这与中国电信、中国联通、中国移动国际出口带宽和线路不同有关，所以以实测为准。可以先选定一个服务器位置来按照教程进行搭建，熟悉搭建方法，当账号搭建完成并进行了bbr加速后，测试下速度自己是否满意，如果满意那就用这个服务器位置的服务器。如果速度不太满意，就一次性开几台不同的服务器位置的服务器，然后按照同样的方法来进行搭建并测试，选择最优的，之后把其它的服务器删掉，按小时计费测试成本可以忽略。

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_2009190906401.png)

这里可以选择举距离国内相对较近的日本、香港、新加坡节点

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_2009190910342.png)

这里注意最新的系统默认是centos8 x64，然后这个最新的centos8的系统在配置ss的时候，会有一些错误，后面我们会记录并给出解决方案，centos6的话就没有什么问题（可以点击图中的CentOS几个字，会弹出centos6，然后选中centos6）。

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_2009190910383.png)

接下来这一步是开启vps的ipv6 ip，选填项。如果你的电脑系统可以用ipv6，那么可以勾选此项。大多数用户没有这个需求，但有一些用户可能会用到，所以补充了这部分内容。

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_2009190916104.png)

## 4.查看服务器信息

开通服务器时，当出现了ip，不要立马去ping或者用xshell去连接，再等5分钟之后，有个缓冲时间。完成购买后，找到系统的密码记下来，部署服务器时需要用到。vps系统（推荐centos6）的密码获取方法如下图：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_2009190918455.png)

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_2009190918496.png)

如果你开启了vps的ipv6，那么在后台的settings选项可以找到服务器的ipv6 ip。在部署SSR账号时，你用ipv6 ip就行。整个部署及使用过程中，记得把电脑系统开启ipv6喔。

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_2009190918527.png)

删掉服务器步骤如下图：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_2009190918558.png)

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_2009190918589.png)

一个被墙ip的vps被删掉后，其ip并不会消失，会随机分配给下一个在这个服务器位置新建服务器的人，这就是为什么开新服务器会有一定几率开到被墙的ip。被墙是指在国内地区无法ping通服务器，但在国外是可以ping通的，vultr是面向全球服务，如果这个被墙ip被国外的人开到了，它是可以被正常使用的，半年或1年后这个被墙的ip可能会被国内防火墙解封，那么这就是一个良性循环。

# 第二步：部署VPS服务器

购买服务器后，需要部署一下。因为你买的是虚拟东西，而且又远在国外，我们需要一个叫Xshell的软件来远程部署。

## 1.Xshell下载

Xshell windows版官网下载地址：https://www.netsarang.com/zh/xshell/

学生可以下载免费版，直接输入你的邮箱，xshell就会把免费版下载链接发送到邮箱里：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909244210.png)

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909244611.png)

Xshell Windows版安装文件百度云下载地址：

链接: https://pan.baidu.com/s/1pnorPZRAuOannJX13ulzzQ 提取码: ssbm

如果你是苹果电脑操作系统，更简单，无需下载xshell，系统可以直接连接VPS。打开终端（Terminal），输入ssh root@ip 其中“ip”替换成你VPS的ip, 按回车键，然后复制粘贴密码，按回车键即可登录。粘贴密码时有可能不显示密码，但不影响， [参考设置方法](http://www.cnblogs.com/ghj1976/archive/2013/04/19/3030159.html) 如果不能用MAC自带的终端连接的话，直接网上搜“MAC连接SSH的软件”，有很多，然后通过软件来连接vps服务器就行，具体操作方式参考windows xshell。

## 2.连接购买的服务器

下载windows xshell软件并安装后，打开软件

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909272912.png)

选择文件，新建

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909273213.png)

随便取个名字，然后把你的服务器ip填上

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909273614.png)

连接国外ip即服务器时，软件会先后提醒你输入用户名和密码，用户名默认都是root，密码是你购买的服务器系统的密码。

**如果xshell连不上服务器，没有弹出让你输入用户名和密码的输入框，表明你开到的ip是一个被墙的ip，遇到这种情况，重新开新的服务器，直到能用xshell连上为止，耐心点哦！如果同一个地区开了多台服务器还是不行的话，可以换其它地区。**

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909273815.png)

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909274116.png)

连接成功后，会出现如上图所示，之后就可以复制粘贴代码部署了。

## 3.一键部署

**（CentOs8在后续需要再配置一下，但也需要这些前提步骤，CentOs6应该可以直接配置成功）**CentOS6/Debian6/Ubuntu14 ShadowsocksR一键部署管理脚本（2018.11.21更新）：

**脚本一（2018.11.20更新）：**

yum -y install wget

wget -N — no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh

**备用脚本二（2018.11.21更新）：**

如果上面的脚本暂时用不了，可以用下面的备用脚本，备用脚本没有单独做图文教程，自己摸索下就会了。备用脚本卸载命令：./shadowsocksR.sh uninstall

yum -y install wget

wget — no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh

chmod +x shadowsocksR.sh

./shadowsocksR.sh 2>&1 | tee shadowsocksR.log

--------------------

复制上面的脚本一代码到VPS服务器里，复制代码用鼠标右键的复制，然后在vps里面右键粘贴进去，因为ctrl+c和ctrl+v无效。接着按回车键，脚本会自动安装，以后只需要运行这个快捷命令就可以出现下图的界面进行设置，快捷管理命令为：bash ssr.sh

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909342617.png)

如上图出现管理界面后，输入数字1来安装SSR服务端。如果输入1后不能进入下一步，那么请退出xshell，重新连接vps服务器，然后输入快捷管理命令bash ssr.sh 再尝试。

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909343018.png)

根据上图提示，依次输入自己想设置的端口和密码 (密码建议用复杂点的字母组合，端口号为40–65535之间的数字)，回车键用于确认

注：关于端口的设置，总的网络总端口有6万多个，理论上可以任意设置，但不要以0开头！但是有的地区需要设置特殊的端口才有效，一些特殊的端口比如80、143、443、1433、3306、3389、8080。

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909343419.png)

如上图，选择想设置的加密方式，比如10，按回车键确认

接下来是选择协议插件，如下图：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909343720.png)

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909344021.png)

选择并确认后，会出现上图的界面，提示你是否选择兼容原版，这里的原版指的是SS客户端（SS客户端没有协议和混淆的选项），可以根据需求进行选择，演示选择y

之后进行混淆插件的设置。

注意：如果协议是origin，那么混淆也必须是plain；如果协议不是origin，那么混淆可以是任意的。有的地区需要把混淆设置成plain才好用。因为混淆不总是有效果，要看各地区的策略，有时候不混淆（plain）或者（origin和plain一起使用），让其看起来像随机数据更好。（特别注意：tls 1.2_ticket_auth容易受到干扰！请选择除tls开头以外的其它混淆！！！）

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909344322.png)

进行混淆插件的设置后，会依次提示你对设备数、单线程限速和端口总限速进行设置，默认值是不进行限制，个人使用的话，选择默认即可，即直接敲回车键。

注意：关于限制设备数，这个协议必须是非原版且不兼容原版才有效，也就是必须使用SSR协议的情况下，才有效！

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909344623.png)

之后代码就正式自动部署了，到下图所示的位置，提示你下载文件，输入：y

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909345024.png)

**（这里，如果你的系统是CentOs8，请跳至步骤4.CentOs8安装SSR踩坑）**耐心等待一会，出现下面的界面即部署完成：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909345325.png)

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909345626.png)

根据上图就可以看到自己设置的SSR账号信息，包括IP、端口、密码、加密方式、协议插件、混淆插件，这些信息需要填入你的SSR客户端。如果之后想修改账号信息，直接输入快捷管理命令：bash ssr.sh 进入管理界面，选择相应的数字来进行一键修改。例如：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909345927.png)

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091909350228.png)

脚本演示结束。

此脚本是开机自动启动，部署一次即可。最后可以重启服务器确保部署生效（一般情况不重启也可以）。重启需要在命令栏里输入reboot ，输入命令后稍微等待一会服务器就会自动重启，一般重启过程需要2～5分钟，重启过程中Xshell会自动断开连接，等VPS重启好后才可以用Xshell软件进行连接。如果部署过程中卡在某个位置超过10分钟，可以用xshell软件断开，然后重新连接你的ip，再复制代码进行部署。

## 4.CentOs8安装SSR踩坑

**安装完成，ssr服务启动失败：**

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091910361129.png)

对CentOs系统进行参数配置，配置完成启动的时候可能会**失败**，这时参考下面的问题。 需要注意的几个问题是：

### 1.Python 导致的启动失败

Vultr 上买的新服务器(centOS 8)虽然已经安装了 python36(若服务器没有安装 python 则需要先安装 python)，但没有配置为默认，因此 ssr 客户端会启动失败。

因此要先配置 Python36 为默认 Python：

```
alternatives --set python /usr/bin/python3
```

至此 ssr 客户端就可以正常启动了。

### 2.firewall 导致的无法连接（适用于配好了SSR无法上网的情况）

第二：服务器默认开启了 firewall 防火墙，且默认没有开放任何端口，因此要将所配置的 ssr 端口(以443为例)添加到白名单并重启防火墙

```
firewall-cmd --add-port=443/tcp --permanent
firewall-cmd --add-port=443/udp --permanent
firewall-cmd --reload
```

然后就可以正常连接和使用 ssr 进行科学上网活动了。

### 3.ssr管理

使用的配置

```
加密：aes-256-cfb
协议：origin
混淆：http_post
```

设备数限制和线程数限制可自行斟酌，不需要可以直接按回车跳过。具体关于协议和混淆的配置请参考文末的参考文章。

要启动 ssr 一键脚本管理，可以运行

```
bash ssr.sh
```

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091910374730.png)

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091910384831.png)

# 第三步：一键加速VPS服务器

等有时间再搞，急用请移步：[https://medium.com/@tyrr31186065/vultr%E8%87%AA%E5%BB%BAss%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%95%99%E7%A8%8B-7426b117361](https://medium.com/@tyrr31186065/vultr自建ss服务器教程-7426b117361)

# 第四步：SSR客户端下载与使用

第一次电脑系统使用SSR/SS客户端时，如果提示你需要安装NET Framework 4.0，网上搜一下这个东西，安装一下即可。NET Framework 4.0是SSR/SS的运行库，没有这个SSR/SS客户端无法正常运行。有的电脑系统可能会自带NET Framework 4.0。

Windows SSR客户端 [下载地址](https://github.com/shadowsocksr-backup/shadowsocksr-csharp/releases) [备用下载地址](https://nofile.io/f/6Jm7WJCyOVv/ShadowsocksR-4.7.0-win.7z)

MAC SSR客户端 [下载地址](https://github.com/shadowsocksr-backup/ShadowsocksX-NG/releases) [备用下载地址](https://nofile.io/f/jgMWFwCBonU#ab0d3c3b6ac54482)

[Linux客户端一键安装配置使用脚本(使用方法见注释)](https://github.com/the0demiurge/CharlesScripts/blob/master/charles/bin/ssr) 或者采用图形界面的[linux ssr客户端](https://github.com/erguotou520/electron-ssr/releases)

安卓SSR客户端 [下载地址](https://github.com/shadowsocksr-backup/shadowsocksr-android/releases/download/3.4.0.8/shadowsocksr-release.apk) [备用下载地址](https://nofile.io/f/rvTJoj0h5GC/shadowsocksr-release.apk)

苹果手机SSR客户端：Potatso Lite、Potatso、shadowrocket都可以作为SSR客户端，但这些软件目前已经在国内的app商店下架，可以用美区的appid账号来下载。但是，如果你配置的SSR账号兼容SS客户端，或者协议选择origin且混淆选择plain，那么你可以选择苹果SS客户端软件（即协议和混淆可以不填）。在大陆app商店里面可以尝试搜索：Wingy、shadowsocks，如果软件都被下架了，建议自己注册美区appid来下载，软件多的很！[苹果手机申请美区apple id方法](https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd=苹果手机如何申请美区apple id&oq=%E8%8B%B9%E6%9E%9C%E6%89%8B%E6%9C%BA%E5%A6%82%E4%BD%95%E6%B3%A8%E5%86%8C%E7%BE%8E%E5%8C%BAapple%20id&rsv_pq=9b0ef06900045aac&rsv_t=a6daySwnrXFrSrC%2BIlgLIeU321j1oRm%2F%2FJgdL3RAdT6GSkIIcOaBGKnfvjE&rqlang=cn&rsv_enter=0&inputT=2113&rsv_sug3=54&rsv_sug2=0&rsv_sug4=2440&rsv_sug=1)。

有了账号后，打开SSR客户端，填上信息，这里以windows版的SSR客户端为例子：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091910400932.png)

在对应的位置，填上服务器ip、服务器端口、密码、加密方式、协议和混淆，最后将浏览器的代理设置为（http）127.0.0.1和1080即可。账号的端口号就是你自己设置的，而要上网的浏览器的端口号是1080，固定的，谷歌浏览器可以通过 SwitchyOmega 插件来设置。

启动SSR客户端后，右键SSR客户端图标，选择第一个“系统代理模式”，里面有3个子选项，选择”全局模式“，之后就可以用浏览器设置好了的代理模式（http）127.0.0.1和1080翻墙，此模式下所有的网站都会走SSR代理。

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091910401233.png)

### **常见问题参考解决方法：**

**1、用了一段时间发现ssr账号用不了了？**

首先ping一下自己的ip，看看能不能ping的通，ping不通那么就是ip被墙了，ip被墙时，xshell也会连接不上服务器，遇到这种情况重新部署一个新的服务器，新的服务器就是新的ip。关于怎么ping ip的方法，可以自行网上搜索，或者用xshell软件连接服务器来判断，连不上即是被墙了。vultr开通和删除服务器非常方便，新服务器即新ip，大多数vps服务商都没有这样的服务，一般的vps服务商可能会提供免费更换1次ip的服务。

**2、刚搭建好的ssr账号，ip能ping通，但是还是用不了？**

首选排除杀毒软件的干扰，尤其是国产杀毒软件，比如360安全卫生、360杀毒软件、腾讯管家、金山卫生等。这些东西很容易干扰翻墙上网，如果你的电脑安装了这样的东西，建议至少翻墙时别用，最好卸载。其次，检查下SSR信息是否填写正确。浏览器的代理方式是否是ssr代理，即（HTTP）127.0.0.1 和1080。如果以上条件都排除，还是用不了，那么可以更换端口、加密方式、协议、混淆，或者更换服务器位置。另外，如果你的vps服务器配置的是SSR账号，即有协议和混淆且没有兼容原版(SS版），那么你必须使用SSSR客户端来使用账号，因为SS客户端没有填写协议和混淆的选项。

**3、有的地区需要把混淆参数设置成plain才好用。**因为混淆不总是有效果，要看各地区的策略，有时候不混淆（plain）让其看起来像随机数据更好。

**4、电脑能用但手机用不了？**

如果你的手机用的是SS客户端，SS客户端没有填协议和混淆的地方，如果你部署的协议和混淆的时候没有选择兼容原版（SS版），因此手机是用不了的。这个时候你把协议弄成兼容原版、混淆也设置成兼容原版即可。或者直接将协议设置成origin且混淆设置成plain。

**5、vps的服务器操作系统不要用的太高**，太高可能会因为系统的防火墙问题导致搭建的SSR账号连不上。如果某个系统不好用，可以选择其它的系统来尝试。

**6、vultr服务商提供的vps服务器是单向流量计算**，有的vps服务商是双向流量计算，单向流量计算对于用户来说更实惠。因为我们是在vps服务器上部署SSR服务端后，再用SSR客户端翻墙，所以SSR服务端就相当于中转，比如我们看一个视频，必然会产生流量，假如消耗流量80M，那么VPS服务器会产生上传80M和下载80M流量，vultr服务商只计算单向的80M流量。如果是双向计算流量，那么会计算为160M流量。

**7、如果你想把搭建的账号给多人使用**，不用额外设置端口，因为一个账号就可以多人使用。一般5美元的服务器可以同时支持40人在线使用。

如果想实现支持每个用户(端口)不同的加密方式/协议/混淆等，并且管理流量使用，可以参考多用户配置脚本：wget -N — no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssrmu.sh && chmod +x ssrmu.sh && bash ssrmu.sh 安装后管理命令为：bash ssrmu.sh

注意：这个多用户配置脚本和教程内容的脚本无法共存！要想用这个脚本，把之前的脚本卸载，输入管理命令bash ssr.sh ，选择3，卸载ShadowsocksR即可卸载原脚本。

**8、vultr服务器每月有流量限制**，超过限制后服务器不会被停止运行，但是超出的流量会被额外收费。北美和西欧地区的服务器超出流量后，多出的部分收费为0.01美元/G。新加坡和日本东京（日本）为0.025美元/G，悉尼（澳大利亚）为0.05美元/G。把vultr服务器删掉，开通新的服务器，流量会从0开始重新计算。

**9、vultr怎样才能申请退款呢？**

vultr和其他的国外商家一样，都是使用工单的形式与客服联系，如果需要退款，直接在后台点击support，选择open ticket新开一个工单，选择billing question财务问题，简单的在文本框输入你的退款理由。比如：Please refund all the balance in my account。工单提交以后一般很快就可以给你确认退款，若干个工作日后就会退回你的支付方式。（全额退款结束后，账号可能会被删除）

如果英语水平不好，但是想和客服进行交流，可以用百度在线翻译，自动中文转英文和英文转中文。

**10、路由器也可以配置ss/ssr账号**，详见openwrt-ssr项目地址：https://github.com/ywb94/openwrt-ssr

**11、如果电脑想用搭建的ss/ssr账号玩游戏**，即实现类似VPN全局代理，可以用SSTAP，教程：https://www.jianshu.com/p/519e68b74646

**12、配置bbr加速脚本**，重启电脑后xshell无法连接服务器。如果你遇到这样的问题，只能把服务器删除了，重新搭建个新的，可以先配置bbr加速脚本再配置ss/ssr脚本。

# 第五步：校园网ipv6免流配置【需要登陆】

## 1.检查所在环境是否可以使用ipv6(插网线不登录)

https://ipv6-test.com/
https://test-ipv6.com/
https://bt.byr.cn/login.php

## 2.ss使用ipv6【亲测新版ssr好像不需要配置就可以打通】

保险起见，这里还是说一下新旧版配置：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091911010334.png)

可以看到新版的自动带有ipv6的检测：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091911022335.png)

旧版修改方式：

注意修改config.json中的必要参数，**注意`server`参数需要设定为`"::"`才能使用IPv6**。

- 启动shadowsocks：`/etc/init.d/shadowsocks start`。

**示例config.json：**

```
{
    "server":"::",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

然后重启ssr即可。

## 3.在客户端ssr里如同ipv4那样配置，只是ip地址换成ipv6的地址

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20091911101536.png)

切换到校园网，切换代理为全局模式，即可白嫖上网，但这种需要先连接校园网登入账号。

测试：开启全局模式，同时打开油管和B站持续至少20分钟，等20分钟再看流量的消耗，约2MB。服务器流量使用0.03GB，单独全局开油管至少2个半小时之后呢？消耗约2.7GB，而校园网流量消耗约29MB（与我退出全局模式后短时开启一系列网址有关），一切正常。

【注意！！！】只是这种方式只能上网白嫖，不能使用应用

# 第六步：实现不登陆校园网通过纯ipv6代理上网

配置完步骤五，我们会发现，在不登陆校园网账号的情况下只可以免流访问部分支持ipv6的网站，而要访问只支持ipv4的网站，则还是需要登陆校园网账号，此时，会出现校园网流量偷跑的情况。

那么如何实现不登陆校园网实现全部网站免流上网呢？

**答：IPv6 ONLY VPS访问IPv4资源**

```
步骤：
1.在VPS上配置好传输代理程序。
2.禁用你校园网本机的IPv4协议。
3.通过校园网的IPv6地址与你VPS上的IPv6地址建立连接。（根据不同服务端选择相应客户端）
```

具体操作：

1.完成步骤五，在VPS上配置好SSR

2.修改你的VPS的DNS服务器到DNS64提供者的DNS

```
DNS64是与NAT64搭配使用的，原理很简单，修改你的DNS到DNS64提供者的DNS，当你发出向解析到IPv4的域名的请求后，DNS会将IPv4地址按照一定格式嵌入IPv6地址中；这个返回IPv6地址会指向NAT64的服务器，NAT64网关会按照它包含的信息获取IPv4的数据并转发给你，这样一来你就能够直接访问IPv4的网站了。

提供DNS64的服务商很多，比如谷歌等，但是它们并不提供配套的NAT64，需要你自己在内网搭建一个NAT64网关。当然欧洲有一些公益组织提供免费的DNS64+NAT64服务，比如下面这两个。

http://www.trex.fi/2011/dns64.html
2001:67c:2b0::4
2001:67c:2b0::6

https://go6lab.si/current-ipv6-tests/nat64dns64-public-test/
2001:67c:27e4:15::6411
2001:67c:27e4::64

https://nat64.level66.network
2a09:11c0:f1:bbf0::70

一般修改/etc/resolv.conf的namesever值即可，不过部分系统想要永久修改需要编辑一些其他的参数，大家就自行查阅资料吧

DNS64的好处是配置十分方便，足以满足大部分的调试需求。当然弊端也十分明显，服务商会记录你三天的浏览记录以防止用于不法用途，且NAT64服务器到你的服务器速度未必非常理想。
```

3.在你的本地主机上，Settings->Network&Internet->Proxy，开启Proxy，并填入address：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20092111031837.png)

4.控制面板->网络共享中心->网络连接->属性->禁用IPv4协议：

![](https://images.cnblogs.com/cnblogs_com/phoenixash/1850577/o_20092111094738.png)

5.SSR配置ipv6和全局模式，代理规则改为：绕过局域网即可。

## **测试：**

可自由访问Google、知乎、B站、csdn等，但无法访问校园网。

参考：

[https://medium.com/@tyrr31186065/vultr%E8%87%AA%E5%BB%BAss%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%95%99%E7%A8%8B-7426b117361](https://medium.com/@tyrr31186065/vultr自建ss服务器教程-7426b117361)

[https://cheereus.com/2020/02/07/%E5%9C%A8-Vultr-%E7%9A%84-centOS-8-%E4%B8%8A%E5%AE%89%E8%A3%85-ssr-%E8%B8%A9%E5%9D%91%E8%AE%B0/](https://cheereus.com/2020/02/07/在-Vultr-的-centOS-8-上安装-ssr-踩坑记/)

https://vultr.gitbook.io/docs/vultr-jiao-cheng

[https://calmcapk.github.io/2019/11/28/windows10-vultr-ssr-ipv6%E6%90%AD%E5%BB%BA%E6%A0%A1%E5%9B%AD%E7%BD%91%E5%85%8D%E6%B5%81%E9%87%8F/](https://calmcapk.github.io/2019/11/28/windows10-vultr-ssr-ipv6搭建校园网免流量/)

https://www.newlearner.site/2018/09/15/use-ipv6-for-free-internet.html#2-2

https://luotianyi.vc/2701.html
