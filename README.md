# 小米Xiaomi BE6500 解锁SSH和科学上网教程
本教程一共分为两个视频，第一个是解锁SSH，第二个是安装ShellCrash科学上网。在小米原来的固件上安装科学上网软件，不影响路由器原有的功能，并且支持全屋科学上网。

## 小米路由器 BE6500 解锁SSH教程
视频教程：▶https://youtu.be/iZNAhoIsJJ4

### 第一步：下载固件和SSH工具
- 小米路由器 BE6500 固件 1.0.29 版本，<a href="https://cdn.cnbj1.fds.api.mi-img.com/xiaoqiang/rom/rn02/miwifi_rn02_firmware_release_1.0.29.bin">点击下载</a>
- 工具下载：<a href="https://github.com/eujc/lyq/releases/download/ROM/putty.zip" target="_blank">Putty下载>></a> ，MAC使用Termius，<a href="https://termius.com/download" target="_blank">点击下载>></a>
- 固件版本需要是1.0.29，如果不是这个版本，请使用 <a href="https://bigota.miwifi.com/xiaoqiang/tools/MIWIFIRepairTool.x86.zip" target="_blank">小米路由器修复工具</a> 完成固件降级，<a href="https://youtu.be/noBqKNq2MTk" target="_blank">降级教程</a>。

### 第二步：解锁SSH
Windows用户可使用 <code>命令提示符</code> 、MacOS用户可使用 <code>终端</code> ，输入下列代码开启小米路由器 BE6500 的SSH功能。<br>
请将 <STOK> 替换成 stok 码，注意：每次登录路由器后台 stok 码会改变；<IP>修改为目前网络的IPV4 IP 地址，默认为 192.168.31.1。，如果IP不正确的话，shell会返回 {"code":401,"msg":"invalid token"}
![image](https://github.com/user-attachments/assets/4ca93655-0ef9-421a-9b7e-cf15eb7c5033)

    
    curl -X POST http://<IP>/cgi-bin/luci/;stok=<STOK>/api/xqsystem/start_binding -d "uid=1234&key=1234'%0Anvram%20set%20ssh_en%3D1'"
    
    curl -X POST http://<IP>/cgi-bin/luci/;stok=<STOK>/api/xqsystem/start_binding -d "uid=1234&key=1234'%0Anvram%20commit'"
    
    curl -X POST http://<IP>/cgi-bin/luci/;stok=<STOK>/api/xqsystem/start_binding -d "uid=1234&key=1234'%0Ased%20-i%20's%2Fchannel%3D.*%2Fchannel%3D%22debug%22%2Fg'%20%2Fetc%2Finit.d%2Fdropbear'"
    
    curl -X POST http://<IP>/cgi-bin/luci/;stok=<STOK>/api/xqsystem/start_binding -d "uid=1234&key=1234'%0A%2Fetc%2Finit.d%2Fdropbear%20start'"

### 第三步：登录路由器
- 工具下载：<a href="https://github.com/uyez/lyq/releases/download/rom/putty.zip">[Putty下载]</a>
- 通过SSH登录，用户名是：root，密码通过网站计算，<a href="https://miwifi.dev/ssh" target="_blank">密码计算网站>></a>
- 更改SSH登录密码，执行以下指令：（修改之后的用户名是：root，密码是：admin）

  echo -e 'admin\nadmin' | passwd root

- 通过SSH登录
 <img width="850" alt="image" src="https://github.com/user-attachments/assets/0e2f672e-fd99-46f4-9460-1a2f5a2e3a45" />
  登录成功：
  <img width="853" alt="image" src="https://github.com/user-attachments/assets/60595eb3-b21c-412f-9662-d9fb57e4137e" />


### 第四步：固化SSH
- 用SSH工具登录路由器后分别执行以下指令，每次执行指令后路由器会重启。

    nvram set ssh_en=1
    nvram set telnet_en=1
    nvram set uart_en=1
    nvram set boot_wait=on
    nvram commit
  
- 执行以下代码，添加自动开启 SSH 端口指令
    mkdir /data/auto_ssh && cd /data/auto_ssh
    curl -O https://fastly.jsdelivr.net/gh/lemoeo/AX6S@main/auto_ssh.sh
    chmod +x auto_ssh.sh

    uci set firewall.auto_ssh=include
    uci set firewall.auto_ssh.type='script'
    uci set firewall.auto_ssh.path='/data/auto_ssh/auto_ssh.sh'
    uci set firewall.auto_ssh.enabled='1'
    uci commit firewall


### 第五步：安装ShellClash
视频教程： https://youtu.be/Z9hr7bGhA7M

Clash安装源：

export url='https://fastly.jsdelivr.net/gh/juewuy/ShellCrash@master' && sh -c "$(curl -kfsSl $url/install.sh)" && source /etc/profile &> /dev/null
备用安装源：

export url='https://gh.jwsc.eu.org/master' && sh -c "$(curl -kfsSl $url/install.sh)" && source /etc/profile &> /dev/null
Clash管理地址： http://192.168.31.1:9999/ui/ (如果打不开请按Ctrl+F5 刷新)

### 第六步：对shellclash配置
![FireShot Capture 063 - 草东日记 - 小米路由器BE6500 Pro · 解锁SSH 启用科学上网 -  www gaicas com _00](https://github.com/user-attachments/assets/69bbbb14-5a38-4ec9-b160-c20341952b31)
![FireShot Capture 063 - 草东日记 - 小米路由器BE6500 Pro · 解锁SSH 启用科学上网 -  www gaicas com _01](https://github.com/user-attachments/assets/48bba106-b1be-46f4-8c56-83a930417f5b)
![FireShot Capture 063 - 草东日记 - 小米路由器BE6500 Pro · 解锁SSH 启用科学上网 -  www gaicas com _02](https://github.com/user-attachments/assets/ceab306b-422d-4396-b256-0236e73ee506)
![FireShot Capture 063 - 草东日记 - 小米路由器BE6500 Pro · 解锁SSH 启用科学上网 -  www gaicas com _03](https://github.com/user-attachments/assets/b7096e1b-c692-4b1e-a505-a4733d691574)
