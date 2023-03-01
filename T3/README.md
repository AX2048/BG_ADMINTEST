iptables -A OUTPUT -p tcp --dport 3306 -j LOG --log-prefix "MySQL Outgoing: "

tail -f /var/log/syslog | grep "MySQL Outgoing"

iptables-save > /etc/iptables/rules.v4
