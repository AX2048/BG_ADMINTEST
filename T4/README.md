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

### Terminal

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

### File

crontab 

```$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
# You can also override PATH, but by default, newer versions inherit it from the environment
#PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```

Example
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
