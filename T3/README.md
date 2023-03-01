## Задача 3

Напишите правило в iptables для логирования исходящих запросов на порт 3306.

---

Чтобы регистрировать все исходящие запросы на порт 3306:

```
iptables -A OUTPUT -p tcp --dport 3306 -j LOG --log-prefix "MySQL Outgoing: "
```

`-A` новое правило в конец цепочки OUTPUT;
`-p tcp` фильтрует TCP-пакеты;
`--dport` указать прот, в нашем случае стандартный `3306`;
`--log-prefix` добавляет префикс к сообщению журнала, в нашем случае `MySQL Outgoing: `

Просмотреть журналы:

```
tail -f /var/log/syslog | grep "MySQL Outgoing"
```

Сохранить правила iptables, чтобы они сохранялись при перезагрузке:
```
iptables-save > /etc/iptables/rules.v4
```
