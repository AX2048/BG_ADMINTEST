![](FUNNY.png)

---

### T1: FUNNY PING
## Задача 1

Запусти команду в терминале: ping 1297614856. Прикольно, да?
Какое число нужно указать чтобы аналогично пропинговать 8.8.8.8?
По какой логике получилось найти его?

---

### Флоу

```
$ ping 1297614856
PING 1297614856 (77.88.8.8) 56(84) bytes of data.
64 bytes from 77.88.8.8: icmp_seq=1 ttl=249 time=30.9 ms
64 bytes from 77.88.8.8: icmp_seq=2 ttl=249 time=30.3 ms
```

1297614856 = 77.88.8.8

```
$ ping 134744072
PING 134744072 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=108 time=49.1 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=108 time=51.7 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=108 time=49.1 ms
```

Итого: 8.8.8.8 = 134744072

Логика: 256 = 2^8, приводим к десятичной ситсеме.


### Some fun

```
ponomero@szbewcktse:~$ ping 0xFF
PING 0xFF (0.0.0.255) 56(84) bytes of data.

ponomero@szbewcktse:~$ ping 0
PING 0 (127.0.0.1) 56(84) bytes of data.
```