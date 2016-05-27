## Linux防火墙——iptables

### 设置端口转发

#### 从一台机器转发到另一台机器

* 将访问100.39的9999端口的数据包转发到133.130.107.229的web服务端口上

```shell
    启用ip转发
    echo 1 > /proc/sys/net/ipv4/ip_forward

    配置前路由，将目的端口为9999的数据包转发到133.130.107.229的80端口上
    iptables -t nat -A PREROUTING -d 192.168.100.39 -p tcp --dport 9999 -j DNAT --to 133.130.107.229:80

    配置后路由
    iptables -t nat -A POSTROUTING -j MASQUERADE
    iptables -t nat -A POSTROUTING -p tcp --dport 80  -j MASQUERADE
```

允许流经桥的数据包通过iptables

echo 1 > /proc/sys/net/bridge/bridge-nr-call-iptables

iptables -A FORWARD -p tcp -d 192.168.200.40 --dport 80 -j DROP
iptables -A FORWARD -p tcp -d 192.168.200.40 --dport 80 -j REJECT

http://ebtables.netfilter.org/misc/brnf-faq.html

9:51 87 35 328207 6165213



iptables -t nat -A PREROUTING -d 192.168.100.39 -p tcp --dport 80 -j DNAT --to 133.130.107.229:80

iptables -t nat -A PREROUTING -i eth1 -d 192.168.200.40 -p tcp --dport 80 -j DNAT --to 192.168.200.38:80


iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

iptables -t nat -A POSTROUTING -p tcp --dport 80  -j MASQUERADE

brctl addbr br0
brctl addif br0 eth1

