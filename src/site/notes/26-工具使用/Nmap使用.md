---
{"created":"2024-11-22T10:38:23.136+08:00","tags":["端口扫描"],"Type":"null","dg-publish":true,"aliases":["nmap"],"permalink":"/26-工具使用/Nmap使用/","dgPassFrontmatter":true,"noteIcon":"2"}
---

## 1. 使用
### 1.1. 功能介绍

1、检测活在网络上的主机(主机发现)

2、检测主机上开放的端口(端口发现或枚举)

3、检测到相应的端口(服务发现)的软件和版本

4、检测操作系统，硬件地址，以及软件版本

5、检测脆弱性的漏洞(Nmap的脚本)

### 1.2. 命令参数

1. -sT TCP connect()扫描，这种方式会在目标主机的日志中记录大批连接请求和错误信息。

2. -sS 半开扫描，很少有系统能把它记入系统日志。不过，需要Root权限。

3. -sF -sN 秘密FIN数据包扫描、Xmas Tree、Null扫描模式

4. -sP ping扫描，Nmap在扫描端口时，默认都会使用ping扫描，只有主机存活，Nmap才会继续扫描。

5. -sU UDP扫描，但UDP扫描是不可靠的

6. -sA 这项高级的扫描方法通常用来穿过防火墙的规则集

7. -sV 探测端口服务版本

8. -Pn 扫描之前不需要用ping命令，有些防火墙禁止ping命令。可以使用此选项进行扫描

9. -v 显示扫描过程，推荐使用

10. -h 帮助选项，是最清楚的帮助文档

11. -p 指定端口，如“1-65535、1433、135、22、80”等 -O 启用远程操作系统检测，存在误报

12. -A 全面系统检测、启用脚本检测、扫描等

13. -oN/-oX/-oG 将报告写入文件，分别是正常、XML、grepable 三种格式

14. -T4 针对TCP端口禁止动态扫描延迟超过10ms

15. -iL 读取主机列表，例如，“-iL C:\ip.txt”

16. nmap –iflist : 查看本地主机的接口信息和路由信息

17. -A ：选项用于使用进攻性方式扫描

18. -T4： 指定扫描过程使用的时序，总有6个级别（0-5），级别越高，扫描速度越快，但也容易被防火墙或IDS检测并屏蔽掉，在网络通讯状况较好的情况下推荐使用T4

19. -oX test.xml： 将扫描结果生成 test.xml 文件，如果中断，则结果打不开

20. -oA test.xml: 将扫描结果生成 test.xml 文件，中断后，结果也可保存

21. -oG test.txt: 将扫描结果生成 test.txt 文件

22. -sn : 只进行主机发现，不进行端口扫描

23. -O : 指定Nmap进行系统版本扫描

24. -sV: 指定让Nmap进行服务版本扫描

25. -p : 扫描指定的端口 -sS/sT/sA/sW/sM:指定使用 TCP SYN/Connect()/ACK/Window/Maimon scans的方式来对目标主机进行扫描

26. -sU: 指定使用UDP扫描方式确定目标主机的UDP端口状况

27. -script

28. -sL: List Scan 列表扫描，仅将指定的目标的IP列举出来，不进行主机发现 -sY/sZ: 使用SCTP INIT/COOKIE-ECHO来扫描SCTP协议端口的开放的情况

29. -sO: 使用IP protocol 扫描确定目标机支持的协议类型

30. -PO : 使用IP协议包探测对方主机是否开启

31. -PE/PP/PM : 使用ICMP echo、 ICMP timestamp、ICMP netmask 请求包发现主机

32. -PS/PA/PU/PY : 使用TCP SYN/TCP ACK或SCTP INIT/ECHO方式进行发现

33. -sN/sF/sX: 指定使用TCP Null, FIN, and Xmas scans秘密扫描方式来协助探测对方的TCP端口状态

34. -e eth0：指定使用eth0网卡进行探测

35. -f : –mtu : 指定使用分片、指定数据包的 MTU.

36. -b : 使用FTP bounce scan扫描方式

37. -g： 指定发送的端口号

38. -r: 不进行端口随机打乱的操作（如无该参数，nmap会将要扫描的端口以随机顺序方式扫描，以让nmap的扫描不易被对方防火墙检测到）

39. -v 表示显示冗余信息，在扫描过程中显示扫描的细节，从而让用户了解当前的扫描状态

40. -n : 表示不进行DNS解析；

41. -D <decoy1,decoy2[,ME],…>: 用一组 IP 地址掩盖真实地址，其中 ME 填入自己的 IP 地址

42. -R ：表示总是进行DNS解析。

43. -F : 快速模式，仅扫描TOP 100的端口

44. -S <IP_Address>: 伪装成其他 IP 地址

45. –ttl : 设置 time-to-live 时间

46. –badsum: 使用错误的 checksum 来发送数据包（正常情况下，该类数据包被抛弃，如果收到回复，说明回复来自防火墙或 IDS/IPS）

47. –dns-servers : 指定DNS服务器

48. –system-dns : 指定使用系统的DNS服务器

49. –traceroute : 追踪每个路由节点

50. –scanflags : 定制TCP包的flags

51. –top-ports :扫描开放概率最高的number个端口

52. –port-ratio : 扫描指定频率以上的端口。与上述–top-ports类似，这里以概率作为参数

53. –version-trace: 显示出详细的版本侦测过程信息

54. –osscan-limit: 限制Nmap只对确定的主机的进行OS探测（至少需确知该主机分别有一个open和closed的端口）

55. –osscan-guess: 大胆猜测对方的主机的系统类型。由此准确性会下降不少，但会尽可能多为用户提供潜在的操作系统

56. –data-length : 填充随机数据让数据包长度达到 Num –ip-options : 使用指定的 IP 选项来发送数据包

57. –spoof-mac <mac address/prefix/vendor name> : 伪装 MAC 地址

58. –version-intensity : 指定版本侦测强度（0-9），默认为7。数值越高，探测出的服务越准确，但是运行时间会比较长。

59. –version-light: 指定使用轻量侦测方式 (intensity 2)

60. –version-all: 尝试使用所有的probes进行侦测 (intensity 9)

61. –version-trace: 显示出详细的版本侦测过程信息


### 1.3. 常用命令举例

1. nmap -sT 192.168.96.4 //TCP连接扫描，不安全，慢

2. nmap -sS 192.168.96.4 //SYN扫描,使用最频繁，安全，快

3. nmap -Pn 192.168.96.4 //目标机禁用ping，绕过ping扫描

4. nmap -sU 192.168.96.4 //UDP扫描,慢,可得到有价值的服务器程序

5. nmap -sI 僵尸ip 目标ip //使用僵尸机对目标机发送数据包

6. nmap -sA 192.168.96.4 //检测哪些端口被屏蔽

7. nmap 192.168.96.4 -p //对指定端口扫描

8. nmap 192.168.96.1/24 //对整个网段的主机进行扫描

9. nmap 192.168.96.4 -oX myscan.xml //对扫描结果另存在myscan.xml

10. nmap -T1~6 192.168.96.4 //设置扫描速度，一般T4足够。

11. nmap -sV 192.168.96.4 //对端口上的服务程序版本进行扫描

12. nmap -O 192.168.96.4 //对目标主机的操作系统进行扫描

13. nmap -sC 192.168.96.4 //使用脚本进行扫描，耗时长

14. nmap -A 192.168.96.4 //强力扫描，耗时长

15. nmap -6 ipv6地址 //对ipv6地址的主机进行扫描

16. nmap -f 192.168.96.4 //使用小数据包发送，避免被识别出

17. nmap –mtu 192.168.96.4 //发送的包大小,最大传输单元必须是8的整数

18. nmap -D <假ip> 192.168.96.4 //发送参杂着假ip的数据包检测

19. nmap –source-port //针对防火墙只允许的源端口

20. nmap –data-length: 192.168.96.4 //改变发生数据包的默认的长度，避免被识别出来是nmap发送的。

21. nmap -v 192.168.96.4 //显示冗余信息(扫描细节)

22. nmap -sn 192.168.96.4 //对目标进行ping检测，不进行端口扫描（会发送四种报文确定目标是否存活,）

23. nmap -sP 192.168.96.4 //仅仅对目标进行ping检测。

24. nmap -n/-p 192.168.96.4 //-n表示不进行dns解析，-p表示要

25. nmap –system-dns 192.168.96.4 //扫描指定系统的dns服务器

26. nmap –traceroute 192.168.96.4 //追踪每个路由节点。

27. nmap -PE/PP/PM: 使用ICMP echo, timestamp, and netmask 请求包发现主机。

28. nmap -sP 192.168.96.4 //主机存活性扫描，arp直连方式。

29. nmap -iR [number] //对随机生成number个地址进行扫描。
## 2. Nmap知识
参考资料： [许多Nmap课程都缺乏的入门理论知识](https://www.bilibili.com/video/BV18kqhYKEPK?vd_source=871b2c20bb0692567849c5f3ef688d85) 

### 2.1. tcp协议三次握手发送数据
tcp的三次握手
![Pasted image 20241218133820|333](https://yurain.oss-cn-chengdu.aliyuncs.com/Obsidian/Pasted%20image%2020241218133820.png)
namp -sT 进行tcp扫描时，进行三次握手后，不会发送多余的数据，而是直接发送 `RST` 中断会话，而正常结束TCP会话是需要发送 `FIN` 的
### 2.2. namp判断目标是否开放
![Pasted image 20241218134412](https://yurain.oss-cn-chengdu.aliyuncs.com/Obsidian/Pasted%20image%2020241218134412.png) 
### 2.3. nmap -sS 目标  （半连接扫描）
-sS 参数会让nmap接受到 `SYN ACK` 后直接发送 `RST` 结束会话，会更加的不容易被记录。
> 正常情况是会发送 `FIN` 来结束会话。而不是 `RST`
> 而且必须要发送 `RST` 不然目标主机会以为你没有收到信息，导致目标主机进行重传 `SYN ACK` 造成资源浪费

![Pasted image 20241218134821](https://yurain.oss-cn-chengdu.aliyuncs.com/Obsidian/Pasted%20image%2020241218134821.png)
因为此发送并没有完成整个握手过程。所以 `-sS` 叫做 `半连接扫描` 或者 `半开扫描` `SYN扫描`


但是此扫描默认需要 `sudo` 权限
![Pasted image 20241218135105](https://yurain.oss-cn-chengdu.aliyuncs.com/Obsidian/Pasted%20image%2020241218135105.png)
### 2.4. UDP扫描
udp扫描很慢。因为目标主机一般不会回复没有负载的UDP包。加上防火墙以及限流的诸多因素
建议与一些参数搭配使用 如
`--top-ports 100` 扫描常用的100个端口

### 2.5. 主机发现
一般进行主机发现是使用 `ping` 命令。
nmap可以使用 `-sn` 命令进行ping扫描。 而且可以用 `192.168.0.1-192.168.100.100` 表示ip范围

> [!warning]
> 当遇到windows服务器需要注意，如WindowsServer 2016 2019 2022这样的版本
> 防火墙默认会屏蔽掉所有的icmp包，如果进行用namp -sn 就会进入死胡同了

此时建议使用 `namp -Pn` （No Ping）但是此命令非常耗时间，因为nmap会假设目标主机是正在运行的。如果目标主机没有运行会导致namp进行重复探测
