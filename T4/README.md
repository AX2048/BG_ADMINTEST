![](SUNDAY.png)

---

### T4: EVERY THIRD SUNDAY
## Задача 4

Напиши крон задание стартующее каждое третье воскресение месяца, скажем в 12:00.

---

Cron задание:

`0 12 * * 0#3`


Где:

- `0`: запуск на 0-й минуте.
- `12`: запуск в 12 часов.
- `*`: Запускать каждый день месяца.
- `*`: запускать каждый месяц.
- `0#3`: запуск в третье воскресенье месяца.

`0#3` указывает, что задача должна запускаться в третий раз в указанный день недели в месяце. `0` это воскресенье.

С этим выражением cron задача будет выполняться в 12:00 каждое третье воскресенье месяца.

---

```
ponomero@szbewcktse:~$ crontab -e

...

ponomero@szbewcktse:~$ crontab -l
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
0 12 * * 0#3 curl -LI http://www.google.com

ponomero@szbewcktse:~$ crontab -r -i 
crontab: really delete ponomero's crontab? (y/n) y

ponomero@szbewcktse:~$ crontab -l
no crontab for ponomero
```

/etc/cron.d

```
SHELL=/bin/bash
HOME=/

0 12 * * 0#3 curl -LI http://www.google.com
```

Структура задания:

`minute` `hour` `day_of_month` `month` `day_of_week` `command_to_run`

### Some more

```
crontab -l

crontab -r -i
```
