![](IPTABLES.png)

---

### T3: IPTABLES LOG 3306
## Задача 3

Напишите правило в iptables для логирования исходящих запросов на порт 3306.

---

Регистрировать все исходящие запросы на порт 3306:

```
iptables -A OUTPUT -p tcp --dport 3306 -j LOG --log-prefix "MySQL Outgoing: "
```

`-A` новое правило в конец цепочки OUTPUT;

`-p tcp` фильтрует TCP-пакеты;

`--dport` указать прот, в нашем случае стандартный `3306`;

`--log-prefix` добавляет префикс к сообщению журнала, в нашем случае `MySQL Outgoing: `


### Use 

```
ponomero@szbewcktse:~$ sudo iptables -A OUTPUT -p tcp --dport 3306 -j LOG --log-prefix "MySQL Outgoing: "

ponomero@szbewcktse:~$ sudo iptables --line-numbers -L -v -n
Chain INPUT (policy DROP 243K packets, 12M bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1     126K 9277K REJECT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            multiport dports 22 match-set f2b-sshd src reject-with icmp-port-unreachable
2    1142K  703M ufw-before-logging-input  all  --  *      *       0.0.0.0/0            0.0.0.0/0           
3    1142K  703M ufw-before-input  all  --  *      *       0.0.0.0/0            0.0.0.0/0           
4     343K   44M ufw-after-input  all  --  *      *       0.0.0.0/0            0.0.0.0/0           
...

ponomero@szbewcktse:~$ sudo iptables --line-numbers -L -v -n | grep "MySQL Outgoing"
7        0     0 LOG        tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:3306 LOG flags 0 level 4 prefix "MySQL Outgoing: "
```

### Some more

Просмотреть журналы:

```
tail -f /var/log/syslog | grep "MySQL Outgoing"
```

Сохранить правила iptables, чтобы они сохранялись при перезагрузке:
```
iptables-save > /etc/iptables/rules.v4
```