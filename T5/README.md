![](DEMON.png)

---

### T5: DOCKER DEMON
## Задача 5

Поднять второй докер-демон https://github.com/mlosev/altdocker.
Запустить на нём контейнер с adminer-ом. Сделать так, чтобы можно было открыть web-интерфейс adminer-а введя http://localhost:8080 в браузере.
Подключиться из него к базе на хостинге (предварительно создав её на тестовом аккаунте).

---

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

Симлинки:
```
$ ln -s /path/to/file /path/to/symlink

ln -s /home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdocker.sh cli-dae1
ln -s /home/alex/Documents/CODE/BG_AdminTest/T5/altdocker/altdockerd.sh dae1
```