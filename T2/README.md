![](THREAT.png)

---

### T2: COPY: A HIDDEN THREAT
## Задача 2

Есть не пустая директория /home/paideia/dir1, есть не пустая директория /home/paideia/dir2.
```
paideia@testvm:/home/paideia/$ [0] touch dir1/file.txt dir1/.conf
```
Как скопировать содержимое директории dir1 в dir2 одной командой в терминале?

Внимание, в dir2 не должно быть dir1. Результат /home/paideia/dir2/dir1 НЕ корректен.

---

Содержимое:

```
$ ll dir1/
total 8.0K
drwxr-xr-x 2 alex alex 4.0K Mar  1 08:43 .
drwxr-xr-x 4 alex alex 4.0K Mar  1 20:04 ..
-rw-r--r-- 1 alex alex    0 Mar  1 08:43 .conf
-rw-r--r-- 1 alex alex    0 Mar  1 08:43 file.txt
```

```
$ ll dir2/
total 8.0K
drwxr-xr-x 2 alex alex 4.0K Mar  1 08:47 .
drwxr-xr-x 4 alex alex 4.0K Mar  1 20:04 ..
-rw-r--r-- 1 alex alex    0 Mar  1 08:34 nepusta1.txt
-rw-r--r-- 1 alex alex    0 Mar  1 08:34 nepusta2.md
```

**Простое `cp` не копирует скрытй файл `.conf`.**

```
$ cp dir1/* dir2/

$ ll dir2/
total 8.0K
drwxr-xr-x 2 alex alex 4.0K Mar  1 20:13 .
drwxr-xr-x 4 alex alex 4.0K Mar  1 20:04 ..
-rw-r--r-- 1 alex alex    0 Mar  1 20:13 file.txt
-rw-r--r-- 1 alex alex    0 Mar  1 08:34 nepusta1.txt
-rw-r--r-- 1 alex alex    0 Mar  1 08:34 nepusta2.md
```

Но применив магию, можно скопировать и скрытые файлы:
```
$ cp dir1/{*,.*} dir2/

$ ll dir2/
total 8.0K
drwxr-xr-x 2 alex alex 4.0K Mar  1 20:18 .
drwxr-xr-x 4 alex alex 4.0K Mar  1 20:04 ..
-rw-r--r-- 1 alex alex    0 Mar  1 20:18 .conf
-rw-r--r-- 1 alex alex    0 Mar  1 20:18 file.txt
-rw-r--r-- 1 alex alex    0 Mar  1 08:34 nepusta1.txt
-rw-r--r-- 1 alex alex    0 Mar  1 08:34 nepusta2.md
```

А ещё можно так:
```
$ rsync -av --exclude=dir1/* dir1/ dir2/
sending incremental file list
./
.conf
file.txt

sent 177 bytes  received 57 bytes  468.00 bytes/sec
total size is 0  speedup is 0.00

$ ll dir2/
total 8.0K
drwxr-xr-x 2 alex alex 4.0K Mar  1 08:43 .
drwxr-xr-x 4 alex alex 4.0K Mar  1 20:04 ..
-rw-r--r-- 1 alex alex    0 Mar  1 08:43 .conf
-rw-r--r-- 1 alex alex    0 Mar  1 08:43 file.txt
-rw-r--r-- 1 alex alex    0 Mar  1 08:34 nepusta1.txt
-rw-r--r-- 1 alex alex    0 Mar  1 08:34 nepusta2.md
```

Не забудь удалить файлы: `rm dir2/.conf dir2/file.txt`