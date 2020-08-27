# iptables查看

iptables查看表的命令

```
iptables -t filter -L命令列出filter表中的所有规则

-t 表示 指定需要查看的表
-L 表示 显示该表的规则

其他一些可加参数
-v 表示verbose，显示更多的信息
-n 表示不对IP地址进行名称反解（不使用-n 0.0.0.0/0 这样的ip地址会以anywhere显示），直接显示IP地址
--line-numbers   也可以简写--line 把规则标上序号，使用--line后就会出现下图的num属性
-x 表示显示计数器的精确值


一般我们查看列表普遍采用下面的参数命令
iptables -t nat  -L --line -nv 
iptables -t mangle   -L --line -nv 
iptables -t raw  -L --line -nv 
iptables -t filter  -L --line -nv 
```

![image-20200822110612483](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827162925.png)

```
num:规则序号

pkts:对应规则匹配到的报文的个数。

bytes:对应匹配到的报文包的大小总和。

target:规则对应的target，往往表示规则对应的"动作"，即规则匹配成功后需要采取的措施。

prot:表示规则对应的协议，是否只针对某些协议应用此规则。

opt:表示规则对应的选项。

in:表示数据包由哪个接口(网卡)流入，我们可以设置通过哪块网卡流入的报文需要匹配当前规则。

out:表示数据包由哪个接口(网卡)流出，我们可以设置通过哪块网卡流出的报文需要匹配当前规则。

source:表示规则对应的源头地址，可以是一个IP，也可以是一个网段。

destination:表示规则对应的目标地址。可以是一个IP，也可以是一个网段。
```



iptables 中表链规则 对处理数据包 处理的流程顺序

每个链都会有一个默认的规则，默认规则的作用就是当数据包不符合链里面定义的其他规则的时候就采用默认的策略

然后链里面可以定义一些特殊的规则



当一个数据包到来的时候对链中的每个规则进行匹配，如果满足就执行这个规则定义的动作，如果全部检查完后没有匹配到规则就采取默认的规则

当引用的子链中没有规则的时候，那就继续进行匹配



路由器的内网网卡即我们设备的网关，收到我们设备的数据包后，需要对我们的数据包进行转发，根据路由表中的情况选择第一跳的出口网卡   修改目的mac地址 修改完成后对数据包进行发送，这时候我们的第一跳网卡（如果我们是有线上网，那么这个网卡就是ppp0，如果是无线桥接上网，这个网卡就是无线网卡中的一个，下面的规则中wlan1就是无线桥接中的网卡）

此时iptables规则 nat表中的POSTROUTING链中存在规则----如果出口网卡是wlan1，那么就进行nat转换











### nat表中的链

```
root@IseaWind:~# iptables -t nat  -L --line -nv
Chain PREROUTING (policy ACCEPT 92 packets, 13146 bytes)
num   pkts bytes target                prot opt in     out     source               destination         
1      218 13170 REDIRECT              udp  --  *      *       0.0.0.0/0            0.0.0.0/0            udp dpt:53 redir ports 53
2        0     0 REDIRECT              tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:53 redir ports 53
3       93 13198 prerouting_rule       all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom prerouting rule chain */
4       73  3796 zone_lan_prerouting   all  --  br-lan *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */
5        0     0 zone_wan_prerouting   all  --  eth0.2 *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */
6       20  9402 zone_wan_prerouting   all  --  wlan1  *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain INPUT (policy ACCEPT 36 packets, 2258 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 61 packets, 3936 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain POSTROUTING (policy ACCEPT 10 packets, 602 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1      135  7798 postrouting_rule  all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom postrouting rule chain */
2        0     0 zone_lan_postrouting  all  --  *      br-lan  0.0.0.0/0            0.0.0.0/0            /* !fw3 */
3        0     0 zone_wan_postrouting  all  --  *      eth0.2  0.0.0.0/0            0.0.0.0/0            /* !fw3 */
4      125  7196 zone_wan_postrouting  all  --  *      wlan1   0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain MINIUPNPD (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain MINIUPNPD-POSTROUTING (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain postrouting_lan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain postrouting_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain postrouting_wan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain prerouting_lan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain prerouting_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain prerouting_wan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain zone_lan_postrouting (1 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 postrouting_lan_rule  all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom lan postrouting rule chain */

Chain zone_lan_prerouting (1 references)
num   pkts bytes target     prot opt in     out     source               destination         
1       73  3796 prerouting_lan_rule  all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom lan prerouting rule chain */

Chain zone_wan_postrouting (2 references)
num   pkts bytes target                 prot opt in     out     source               destination         
1      125  7196 MINIUPNPD-POSTROUTING  all  --  *      *       0.0.0.0/0            0.0.0.0/0           
2      125  7196 postrouting_wan_rule   all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom wan postrouting rule chain */
3      125  7196 FULLCONENAT            all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_wan_prerouting (2 references)
num   pkts bytes target               prot opt in     out     source               destination         
1       20  9402 MINIUPNPD            all  --  *      *       0.0.0.0/0            0.0.0.0/0           
2       20  9402 prerouting_wan_rule  all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom wan prerouting rule chain */
3       20  9402 FULLCONENAT          all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

```



### mangle表中的链

```
root@IseaWind:~# iptables -t mangle   -L --line -nv
Chain PREROUTING (policy ACCEPT 831 packets, 66239 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain INPUT (policy ACCEPT 385 packets, 33742 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 426 packets, 23095 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 TCPMSS     tcp  --  *      eth0.2  0.0.0.0/0            0.0.0.0/0            tcp flags:0x06/0x02 /* !fw3: Zone wan MTU fixing */ TCPMSS clamp to PMTU
2       83  4316 TCPMSS     tcp  --  *      wlan1   0.0.0.0/0            0.0.0.0/0            tcp flags:0x06/0x02 /* !fw3: Zone wan MTU fixing */ TCPMSS clamp to PMTU

Chain OUTPUT (policy ACCEPT 383 packets, 55306 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain POSTROUTING (policy ACCEPT 809 packets, 78401 bytes)
num   pkts bytes target     prot opt in     out     source               destination   
```

### raw表中的链

```
Chain PREROUTING (policy ACCEPT 1009 packets, 77583 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1      652 41694 zone_lan_helper  all  --  br-lan *       0.0.0.0/0            0.0.0.0/0            /* !fw3: lan CT helper assignment */

Chain OUTPUT (policy ACCEPT 466 packets, 66816 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain zone_lan_helper (1 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 CT         udp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Amanda backup and archiving proto */ udp dpt:10080 CT helper amanda
2        0     0 CT         tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: FTP passive connection tracking */ tcp dpt:21 CT helper ftp
3        0     0 CT         udp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: RAS proto tracking */ udp dpt:1719 CT helper RAS
4        0     0 CT         tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Q.931 proto tracking */ tcp dpt:1720 CT helper Q.931
5        0     0 CT         tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: IRC DCC connection tracking */ tcp dpt:6667 CT helper irc
6        0     0 CT         tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: PPTP VPN connection tracking */ tcp dpt:1723 CT helper pptp
7        0     0 CT         tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: SIP VoIP connection tracking */ tcp dpt:5060 CT helper sip
8        0     0 CT         udp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: SIP VoIP connection tracking */ udp dpt:5060 CT helper sip
9        0     0 CT         udp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: SNMP monitoring connection tracking */ udp dpt:161 CT helper snmp
10       0     0 CT         udp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: TFTP connection tracking */ udp dpt:69 CT helper tftp

```



### filter表中的链

```
root@IseaWind:~# iptables -t filter  -L --line -nv 



Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target          prot opt in     out     source               destination         
1       84  5776 ACCEPT          all  --  lo     *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */
2      453 40040 input_rule      all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom input rule chain */
3      353 33781 ACCEPT          all  --  *      *       0.0.0.0/0            0.0.0.0/0            ctstate RELATED,ESTABLISHED /* !fw3 */
4        0     0 syn_flood       tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp flags:0x17/0x02 /* !fw3 */
5       99  6227 zone_lan_input  all  --  br-lan *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */
6        0     0 zone_wan_input  all  --  eth0.2 *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */
7        1    32 zone_wan_input  all  --  wlan1  *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain FORWARD (policy DROP 0 packets, 0 bytes)
num   pkts bytes target            prot opt in     out     source               destination         
1      535 28022 forwarding_rule   all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom forwarding rule chain */
2      439 22919 FLOWOFFLOAD       all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Traffic offloading */ ctstate RELATED,ESTABLISHED FLOWOFFLOAD
3      439 22919 ACCEPT            all  --  *      *       0.0.0.0/0            0.0.0.0/0            ctstate RELATED,ESTABLISHED /* !fw3 */
4       96  5103 zone_lan_forward  all  --  br-lan *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */
5        0     0 zone_wan_forward  all  --  eth0.2 *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */
6        0     0 zone_wan_forward  all  --  wlan1  *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */
7        0     0 reject            all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target           prot opt in     out     source               destination         
1       84  5776 ACCEPT           all  --  *      lo      0.0.0.0/0            0.0.0.0/0            /* !fw3 */
2      446 72250 output_rule      all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom output rule chain */
3      325 64411 ACCEPT           all  --  *      *       0.0.0.0/0            0.0.0.0/0            ctstate RELATED,ESTABLISHED /* !fw3 */
4        0     0 zone_lan_output  all  --  *      br-lan  0.0.0.0/0            0.0.0.0/0            /* !fw3 */
5        0     0 zone_wan_output  all  --  *      eth0.2  0.0.0.0/0            0.0.0.0/0            /* !fw3 */
6      121  7839 zone_wan_output  all  --  *      wlan1   0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain MINIUPNPD (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain forwarding_lan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain forwarding_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 ACCEPT     all  --  ppp+   *       0.0.0.0/0            0.0.0.0/0           
2        0     0 ACCEPT     all  --  *      ppp+    0.0.0.0/0            0.0.0.0/0           

Chain forwarding_wan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain input_lan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain input_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain input_wan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain output_lan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain output_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain output_wan_rule (1 references)
num   pkts bytes target     prot opt in     out     source               destination         

Chain reject (5 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 REJECT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */ reject-with tcp-reset
2        0     0 REJECT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */ reject-with icmp-port-unreachable

Chain syn_flood (1 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 RETURN     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp flags:0x17/0x02 limit: avg 25/sec burst 50 /* !fw3 */
2        0     0 DROP       all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_lan_dest_ACCEPT (4 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 ACCEPT     all  --  *      br-lan  0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_lan_forward (1 references)
num   pkts bytes target                 prot opt in     out     source               destination         
1       96  5103 forwarding_lan_rule    all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom lan forwarding rule chain */
2       96  5103 zone_wan_dest_ACCEPT   all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Zone lan to wan forwarding policy */
3        0     0 ACCEPT                 all  --  *      *       0.0.0.0/0            0.0.0.0/0            ctstate DNAT /* !fw3: Accept port forwards */
4        0     0 zone_lan_dest_ACCEPT   all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_lan_input (1 references)
num   pkts bytes target                  prot opt in     out     source               destination         
1       99  6227 input_lan_rule          all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom lan input rule chain */
2        0     0 ACCEPT                  all  --  *      *       0.0.0.0/0            0.0.0.0/0            ctstate DNAT /* !fw3: Accept port redirections */
3       99  6227 zone_lan_src_ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_lan_output (1 references)
num   pkts bytes target                     prot opt in     out     source               destination         
1        0     0 output_lan_rule            all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom lan output rule chain */
2        0     0 zone_lan_dest_ACCEPT       all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_lan_src_ACCEPT (1 references)
num   pkts bytes target     prot opt in     out     source               destination         
1       99  6227 ACCEPT     all  --  br-lan *       0.0.0.0/0            0.0.0.0/0            ctstate NEW,UNTRACKED /* !fw3 */

Chain zone_wan_dest_ACCEPT (2 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 DROP       all  --  *      eth0.2  0.0.0.0/0            0.0.0.0/0            ctstate INVALID /* !fw3: Prevent NAT leakage */
2        0     0 ACCEPT     all  --  *      eth0.2  0.0.0.0/0            0.0.0.0/0            /* !fw3 */
3        0     0 DROP       all  --  *      wlan1   0.0.0.0/0            0.0.0.0/0            ctstate INVALID /* !fw3: Prevent NAT leakage */
4      217 12942 ACCEPT     all  --  *      wlan1   0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_wan_dest_REJECT (1 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 reject     all  --  *      eth0.2  0.0.0.0/0            0.0.0.0/0            /* !fw3 */
2        0     0 reject     all  --  *      wlan1   0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_wan_forward (2 references)
num   pkts bytes target                 prot opt in     out     source               destination         
1        0     0 MINIUPNPD              all  --  *      *       0.0.0.0/0            0.0.0.0/0           
2        0     0 forwarding_wan_rule    all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom wan forwarding rule chain */
3        0     0 zone_lan_dest_ACCEPT   esp  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Allow-IPSec-ESP */
4        0     0 zone_lan_dest_ACCEPT   udp  --  *      *       0.0.0.0/0            0.0.0.0/0            udp dpt:500 /* !fw3: Allow-ISAKMP */
5        0     0 ACCEPT                 all  --  *      *       0.0.0.0/0            0.0.0.0/0            ctstate DNAT /* !fw3: Accept port forwards */
6        0     0 zone_wan_dest_REJECT   all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_wan_input (2 references)
num   pkts bytes target               prot opt in     out     source               destination         
1        1    32 input_wan_rule       all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom wan input rule chain */
2        0     0 ACCEPT               udp  --  *      *       0.0.0.0/0            0.0.0.0/0            udp dpt:68 /* !fw3: Allow-DHCP-Renew */
3        0     0 ACCEPT               icmp --  *      *       0.0.0.0/0            0.0.0.0/0            icmptype 8 /* !fw3: Allow-Ping */
4        1    32 ACCEPT               2    --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Allow-IGMP */
5        0     0 DROP                 tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:8118 /* !fw3: adblock */
6        0     0 ACCEPT               tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:1688 /* !fw3: kms */
7        0     0 ACCEPT               tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:1723 /* !fw3: pptp */
8        0     0 ACCEPT               47   --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: gre */
9        0     0 ACCEPT               all  --  *      *       0.0.0.0/0            0.0.0.0/0            ctstate DNAT /* !fw3: Accept port redirections */
10       0     0 zone_wan_src_REJECT  all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_wan_output (2 references)
num   pkts bytes target                prot opt in     out     source               destination         
1      121  7839 output_wan_rule       all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3: Custom wan output rule chain */
2      121  7839 zone_wan_dest_ACCEPT  all  --  *      *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

Chain zone_wan_src_REJECT (1 references)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 reject     all  --  eth0.2 *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */
2        0     0 reject     all  --  wlan1  *       0.0.0.0/0            0.0.0.0/0            /* !fw3 */

```









openwrt中一个网络就是一个虚拟网卡，虚拟网卡下面统一管理各种接口（这些接口包括物理接口和物理接口产生的虚拟接口）



有线网口是一个接口，一个无线信号是一个接口，路由器中的一个无线网卡可以生成多个无线信号

```
root@IseaWind:~# route 
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         192.168.1.1     0.0.0.0         UG    0      0        0 wlan1
192.168.1.0     *               255.255.255.0   U     0      0        0 wlan1
192.168.11.0    *               255.255.255.0   U     0      0        0 br-lan


root@IseaWind:~# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 wlan1
192.168.1.0     0.0.0.0         255.255.255.0   U     0      0        0 wlan1
192.168.11.0    0.0.0.0         255.255.255.0   U     0      0        0 br-lan


```

![image-20200823220918160](https://raw.githubusercontent.com/yusenyi123/pictures1/master/imgs/20200827162936.png)



route 命令的输出项说明

输出项 说明

| Destination | 目标网段或者主机                                             |
| ----------- | ------------------------------------------------------------ |
| Gateway     | 网关地址，”*”和0.0.0.0 表示目标是本主机所属的网络，不需要路由 |
| Genmask     | 网络掩码                                                     |
| Flags       | 标记。一些可能的标记如下：                                   |
|             | U — 路由是活动的                                             |
|             | H — 目标是一个主机                                           |
|             | G — 路由指向网关                                             |
|             | R — 恢复动态路由产生的表项                                   |
|             | D — 由路由的后台程序动态地安装                               |
|             | M — 由路由的后台程序修改                                     |
|             | ! — 拒绝路由                                                 |
| Metric      | 路由距离，到达指定网络所需的中转数（linux 内核中没有使用）   |
| Ref         | 路由项引用次数（linux 内核中没有使用）                       |
| Use         | 此路由项被路由软件查找的次数                                 |
| Iface       | 该路由表项对应的输出接口                                     |





设置和查看路由表都可以用 route 命令，设置内核路由表的命令格式是：

```
route  [add|del] [-net|-host] target [netmask Nm] [gw Gw] [[dev] If]
```

其中：

- add : 添加一条路由规则
- del : 删除一条路由规则
- -net : 目的地址是一个网络
- -host : 目的地址是一个主机
- target : 目的网络或主机
- netmask : 目的地址的网络掩码
- gw : 路由数据包通过的网关
- dev : 为路由指定的网络接口









## openwrt中的防火墙和iptables的关系

1、OpenWrt做为操作系统，本身就是一个Linux，包管理使用opkg，只是改了一个界面而已。

2、Linux下的防火墙最终都会归iptables进行管理，OpenWrt的防火墙机制同样也是，最上层采用了自己基于UCI标准的配置方法管理防火墙firewall，最终写入到iptables。

3、UCI是OpenWrt统一配置文件的标准，真心不太喜欢这种语法，没iptables来的清晰。

4、OpenWrt基于firewall的配置，由于涉及到多个网口，有Wan和Lan这些，最终会转换成很多自定义的链，看来很吃力，我的建议是直接在firewall层全部开启，然后自己使用iptables做限制。不建议停掉之后再自己写配置，不然你无法知道哪些网口是走向什么地方的。

5、firewall不能单独关闭，不然会无法上网。