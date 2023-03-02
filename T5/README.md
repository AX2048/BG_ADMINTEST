![](DAEMON.png)

---

### T5: DOCKER DAEMON
## Задача 5

Поднять второй докер-демон https://github.com/mlosev/altdocker.
Запустить на нём контейнер с adminer-ом. Сделать так, чтобы можно было открыть web-интерфейс adminer-а введя http://localhost:8080 в браузере.
Подключиться из него к базе на хостинге (предварительно создав её на тестовом аккаунте).

---

### ARCH LINUX WAY
```
$ sh get_docker.sh
+ DOCKER_VERSION=1.12.1
+ '[' '!' -s docker-1.12.1.tgz ']'
+ curl -o docker-1.12.1.tgz https://get.docker.com/builds/Linux/x86_64/docker-1.12.1.tgz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 27.5M  100 27.5M    0     0  5243k      0  0:00:05  0:00:05 --:--:-- 6409k
+ tar -xzvf docker-1.12.1.tgz
docker/
docker/docker-containerd-ctr
docker/docker
docker/docker-containerd
docker/dockerd
docker/docker-proxy
docker/docker-runc
docker/docker-containerd-shim

$ rm ci-dae1 cli-dae1 dae1

$ realpath altdocker.sh 
/home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdocker.sh

$ realpath altdockerd.sh
/home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdockerd.sh

$ ln -s /home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdocker.sh cli-dae1

$ ln -s /home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdockerd.sh dae1

sudo sh dae1
INFO[0000] libcontainerd: new containerd process, pid: 16304 
WARN[0000] containerd: low RLIMIT_NOFILE changing to max  current=1024 max=524288
FATA[0000] can't create unix socket /home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdocker/dae1/exec/libcontainerd/docker-containerd.sock: listen unix /home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdocker/dae1/exec/libcontainerd/docker-containerd.sock: bind: invalid argument 
INFO[0006] libcontainerd: new containerd process, pid: 16336 
WARN[0000] containerd: low RLIMIT_NOFILE changing to max  current=1024 max=524288
FATA[0000] can't create unix socket /home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdocker/dae1/exec/libcontainerd/docker-containerd.sock: listen unix /home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdocker/dae1/exec/libcontainerd/docker-containerd.sock: bind: invalid argument
```
Всё плохо.

### UBUNTU SERVER WAY
**tty1** - запуск докер демона
```
ponomero@szbewcktse:~/altdocker$ sudo apt install bridge-utils

ponomero@szbewcktse:~/altdocker$ sudo apt-get install -y cgroupfs-mount - сомнительное решение

ponomero@szbewcktse:~/altdocker$ sudo apt-get -y install cgroup-tools

ponomero@szbewcktse:~/altdocker$ sudo service docker stop
Warning: Stopping docker.service, but it can still be activated by:
  docker.socket

ponomero@szbewcktse:~/altdocker$ sudo systemctl stop docker.socket

ponomero@szbewcktse:~/altdocker$ sudo service containerd stop

ponomero@szbewcktse:~/altdocker$ sudo cgroupfs-umount
rmdir: failed to remove 'init.scope': Device or resource busy
rmdir: failed to remove 'system.slice': Device or resource busy
rmdir: failed to remove 'user.slice': Device or resource busy

ponomero@szbewcktse:~/altdocker$ sudo cgroupfs-mount
mount: /sys/fs/cgroup/cpuset: cgroup already mounted or mount point busy.
mount: /sys/fs/cgroup/cpu: cgroup already mounted or mount point busy.
mount: /sys/fs/cgroup/blkio: cgroup already mounted on /sys/fs/cgroup/cpuacct.
mount: /sys/fs/cgroup/memory: cgroup already mounted on /sys/fs/cgroup/cpuacct.
mount: /sys/fs/cgroup/pids: cgroup already mounted on /sys/fs/cgroup/cpuacct.

ponomero@szbewcktse:~/altdocker$ sudo bash dae1
INFO[0000] libcontainerd: new containerd process, pid: 1952 
WARN[0000] containerd: low RLIMIT_NOFILE changing to max  current=1024 max=1048576
INFO[0001] Graph migration to content-addressability took 0.00 seconds 
WARN[0001] Your kernel does not support cgroup memory limit 
WARN[0001] Unable to find cpu cgroup in mounts          
WARN[0001] Unable to find blkio cgroup in mounts        
WARN[0001] Unable to find cpuset cgroup in mounts       
WARN[0001] mountpoint for pids not found                
INFO[0001] Loading containers: start.                   
INFO[0001] Firewalld running: false                     

INFO[0001] Loading containers: done.                    
INFO[0001] Daemon has completed initialization          
INFO[0001] Docker daemon                                 commit=23cf638 graphdriver=overlay2 version=1.12.1
INFO[0001] API listen on /home/ponomero/altdocker/altdocker/dae1/docker.sock 
```
Демон успешно запушен и реагирует на наши дейтсвия.

**tty2** - запуск докер клиента (adminer на порту 8080)
```
$ sudo bash cli-dae1 run -p 8080:8080 adminer
docker: Error response from daemon: oci runtime error: rootfs_linux.go:53: mounting "/sys/fs/cgroup" to rootfs "/home/ponomero/altdocker/altdocker/dae1/graph/overlay2/ad5ec5455abb5fed489310c814929e1664d49ed9b9b8e792217320a3d048b573/merged" caused "no subsystem for mount".

ponomero@szbewcktse:~/altdocker$ sudo bash cli-dae1 images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
adminer             latest              dcabc6cf54dd        19 hours ago        250.1 MB

ponomero@szbewcktse:~/altdocker$ sudo bash cli-dae1 ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS               NAMES
10c9bfb3e790        adminer             "entrypoint.sh php -S"   About a minute ago   Created                                 focused_wing


```
Чтож... Пока не работает клиент. Моё альтернативное решение смотри ниже.
### Commands

`sudo bash dae1` - daemon

`sudo bash cli-dae1 run -p 8080:8080 adminer` - cli

### CGROUP

```
ponomero@szbewcktse:~/altdocker$ mount | grep cgroup
cgroup2 on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot)
cgroup on /sys/fs/cgroup/cpuacct type cgroup (rw,relatime,cpuacct)
cgroup on /sys/fs/cgroup/devices type cgroup (rw,relatime,devices)
cgroup on /sys/fs/cgroup/freezer type cgroup (rw,relatime,freezer)
cgroup on /sys/fs/cgroup/net_cls type cgroup (rw,relatime,net_cls)
cgroup on /sys/fs/cgroup/perf_event type cgroup (rw,relatime,perf_event)
cgroup on /sys/fs/cgroup/net_prio type cgroup (rw,relatime,net_prio)
cgroup on /sys/fs/cgroup/hugetlb type cgroup (rw,relatime,hugetlb)
cgroup on /sys/fs/cgroup/rdma type cgroup (rw,relatime,rdma)
cgroup on /sys/fs/cgroup/misc type cgroup (rw,relatime,misc)
```

### Some more
```
sudo service docker stop
sudo service containerd stop
sudo cgroupfs-umount
sudo cgroupfs-mount
sudo service containerd start
sudo service docker start

sudo systemctl stop docker.socket
sudo systemctl stop containerd

mount -t cgroup cgroup /sys/fs/cgroup
```

VPS `ponomero@szbewcktse` IP: 62.113.97.245

```
cat /proc/cgroups | column -t
```

### Рабочее fast решение

Сделать так, чтобы можно было открыть web-интерфейс adminer-а введя http://localhost:8080 в браузере.
Подключиться из него к базе на хостинге (предварительно создав её на тестовом аккаунте).

1. Устанавливаем docker по официальной инсрукции.
2. Запускаем контейнер с adminer на порту 8080:
```
docker run -p 8080:8080 adminer
```

DB connection: (убери 555)
```
ponomag1_adtest

%3kjgUgK555

ponomag1.beget.tech
```

Заходим http://62.113.97.245:8080 и видим:

![](Adminer1.png)
