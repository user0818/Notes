# 简介

Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

## Docker的应用场景

Web 应用的自动化打包和发布。

自动化测试和持续集成、发布。

在服务型环境中部署和调整数据库或其他的后台应用。

从头编译或者扩展现有的 OpenShift 或 Cloud Foundry 平台来搭建自己的 PaaS 环境。

## Docker 的优点

Docker 是一个用于开发，交付和运行应用程序的开放平台。Docker 使您能够将应用程序与基础架构分开，从而可以快速交付软件。借助 Docker，您可以与管理应用程序相同的方式来管理基础架构。通过利用 Docker 的方法来快速交付，测试和部署代码，您可以大大减少编写代码和在生产环境中运行代码之间的延迟。

### 1、快速，一致地交付您的应用程序

Docker 允许开发人员使用您提供的应用程序或服务的本地容器在标准化环境中工作，从而简化了开发的生命周期。

容器非常适合持续集成和持续交付（CI / CD）工作流程，请考虑以下示例方案：

- 您的开发人员在本地编写代码，并使用 Docker 容器与同事共享他们的工作。
- 他们使用 Docker 将其应用程序推送到测试环境中，并执行自动或手动测试。
- 当开发人员发现错误时，他们可以在开发环境中对其进行修复，然后将其重新部署到测试环境中，以进行测试和验证。
- 测试完成后，将修补程序推送给生产环境，就像将更新的镜像推送到生产环境一样简单。

### 2、响应式部署和扩展

Docker 是基于容器的平台，允许高度可移植的工作负载。Docker 容器可以在开发人员的本机上，数据中心的物理或虚拟机上，云服务上或混合环境中运行。

Docker 的可移植性和轻量级的特性，还可以使您轻松地完成动态管理的工作负担，并根据业务需求指示，实时扩展或拆除应用程序和服务。

### 3、在同一硬件上运行更多工作负载

Docker 轻巧快速。它为基于虚拟机管理程序的虚拟机提供了可行、经济、高效的替代方案，因此您可以利用更多的计算能力来实现业务目标。Docker 非常适合于高密度环境以及中小型部署，而您可以用更少的资源做更多的事情。

## Docker组成

Docker 包括三个基本概念:

- **镜像（Image）**：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
- **容器（Container）**：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
- **仓库（Repository）**：仓库可看成一个代码控制中心，用来保存镜像。

Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。

Docker 容器通过 Docker 镜像来创建。

容器与镜像的关系类似于面向对象编程中的对象与类。一个镜像能够创建多个容器。

![d](https://gitee.com/mw515031/image/raw/master/image/image-20210515163510013.png)

## Docker为什么比VM快

1. 因为他有更少的抽象层

2. Docekr利用的是宿主机的内核（启动时不需要重新加载新的OS），VM需要的是Guest OS 

![image-20210517143907842](https://gitee.com/mw515031/image/raw/master/image/image-20210517143907842.png)

# 安装Docker

## 安装流程

```shell
# 1.卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine


# 2.需要的安装包
yum install -y yum-utils

# 3. 设置镜像仓库
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    
# (选做)更新yum软件包索引

# 4.安装docker
yum install docker-ce docker-ce-cli containerd.io

# 5.启动docker
systemctl start docker

# 6.运行hello world镜像
docker run hello-world

# 卸载docker
yum remove docker-ce docker-ce-cli containerd.io
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

配置阿里云docker镜像加速器

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://fojv7gz4.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## Docker run流程

![image-20210517143434002](https://gitee.com/mw515031/image/raw/master/image/image-20210517143434002.png)

# Docker常用命令

https://docs.docker.com/engine/reference/commandline/docker/

## 	帮助命令

```bash
docker version
docker info		#显示docker系统信息
docker --help
```

## 镜像命令

### `docker images` 查看本地主机上的镜像

```bash
[root@localhost ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   2 months ago   13.3kB
```

### `docker search` 搜索dockerhub上的镜像 

```bash
[root@localhost ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10881     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4104      [OK]       
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   808                  [OK]
percona                           Percona Server is a fork of the MySQL relati…   537       [OK]       
……
```

### `docker pull`拉取镜像

```bash
5.7#默认拉取最新版本，可以后接[:tag]指定版本。比如：docker pull mysql:5.7
[root@localhost ~]# docker pull mysql[:tag]
Using default tag: latest	#下载版本
latest: Pulling from library/mysql
69692152171a: Pull complete 	#分层下载，docker image的核心，联合文件系统
1651b0be3df3: Pull complete 	
951da7386bc8: Pull complete 
0f86c95aa242: Pull complete 
37ba2d8bd4fe: Pull complete 
6d278bb05e94: Pull complete 
497efbd93a3e: Pull complete 
f7fddf10c2c2: Pull complete 
16415d159dfb: Pull complete 
0e530ffc6b73: Pull complete 
b0a4a1a77178: Pull complete 
cd90f92aa9ef: Pull complete 
Digest: sha256:d50098d7fcb25b1fcb24e2d3247cae3fc55815d64fec640dc395840f8fa80969	#签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest	#真实下载地址



[root@localhost ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
69692152171a: Already exists 	#如果要下载的某一层已经存在，就不会重复下载
1651b0be3df3: Already exists 
951da7386bc8: Already exists 
0f86c95aa242: Already exists 
37ba2d8bd4fe: Already exists 
6d278bb05e94: Already exists 
497efbd93a3e: Already exists 
a023ae82eef5: Pull complete 
e76c35f20ee7: Pull complete 
e887524d2ef9: Pull complete 
ccb65627e1c3: Pull complete 
Digest: sha256:a682e3c78fc5bd941e9db080b4796c75f69a28a8cad65677c23f7a9f18ba21fa
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

- `docker rmi`删除镜像`-f 可以强制删除已经实例化容器但停止运行的镜像`

```bash
#这里可以指定镜像的id或者名字
[root@localhost ~]# docker rmi -f mysql:5.7
Untagged: mysql:5.7
Untagged: mysql@sha256:a682e3c78fc5bd941e9db080b4796c75f69a28a8cad65677c23f7a9f18ba21fa
Deleted: sha256:2c9028880e5814e8923c278d7e2059f9066d56608a21cd3f83a01e3337bacd68
Deleted: sha256:c49c5c776f1bc87cdfff451ef39ce16a1ef45829e10203f4d9a153a6889ec15e
Deleted: sha256:8345316eca77700e62470611446529113579712a787d356e5c8656a41c244aee
Deleted: sha256:8ae51b87111404bd3e3bde4115ea2fe3fd2bb2cf67158460423c361a24df156b
Deleted: sha256:9d5afda6f6dcf8dd59aef5c02099f1d3b3b0c9ae4f2bb7a61627613e8cdfe562
#这里不会删除所有相关内容，只删除这个镜像特有的分层



#删除所有镜像
[root@localhost ~]# docker rmi -f $(docker images -q)
```

## 容器命令

### `docker run`新建容器 并启动

```bash
docker run [可选参数] image

#参数说明
--name='Name'	指定容器名字
-d				以后台方式运行
-it				交互式操作\终端
-p				指定容器的端口
				-p	ip:主机端口:容器端口
				-p	主机端口:容器端口
				-p	容器端口
-P				(大写P)将容器内部使用的网络端口随机映射到我们使用的主机上
-d				后台运行，不进入容器
```

示例：直接为centos镜像创建一个容器并运行。同时进入交互式操作，并运行`/bin/bash`

```bash
[root@localhost ~]# docker pull centos
Using default tag: latest
latest: Pulling from library/centos
7a0437f04f83: Pull complete 
Digest: sha256:5528e8b1b1719d34604c87e11dcd1c0a20bedf46e83b5632cdeac91b8c04efc1
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest

[root@localhost ~]# docker run -it centos /bin/bash
[root@c96f59bbcae0 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

### `docker ps`查看正在运行的容器

```bash
#-a显示所有容器，包括运行和停止
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED             STATUS                            PORTS     NAMES
c96f59bbcae0   cento         "/bin/bash"   7 minutes ago       Exited (127) About a minute ago             mystifying_banach
75cb88dc0450   d1165f221234   "/hello"      About an hour ago   Exited (0) About an hour ago                amazing_keldysh
8a3e8d73f7b0   d1165f221234   "/hello"      About an hour ago   Exited (0) About an hour ago                boring_murdock
```

### 退出容器

```bash
#退出容器
exit
ctrl+p+q：退出当前的正在交互的容器
```

### 删除容器

```bash
# -f强制删除，即使正在运行
docker rm -f 容器id
docker rm -f $(docker ps -aq)	#删除所有容器
```

### 操作容器

```bash
docker start 容器id
docker restart 容器id
docker stop 容器id
docker kill 容器id
```

ps：`docker stop`和`docker kill`的区别

**Docker Stop主要流程**

1. Docker 通过containerd向容器主进程发送SIGTERM信号后等待一段时间后，如果从containerd收到了容器退出消息那么容器退出成功。

2. 在上一步中，如果等待超时，那么Docker将使用Docker kill 方式试图终止容器

**Docker Kill主要流程**

1. Docker引擎通过containerd使用SIGKILL发向容器主进程，等待一段时间后，如果从containerd收到容器退出消息，那么容器Kill成功
2. 在上一步中如果等待超时，Docker引擎将跳过Containerd自己亲自动手通过kill系统调用向容器主进程发送SIGKILL信号。如果此时kill系统调用返回主进程不存在，那么Docker kill成功。否则引擎将一直死等到containerd通过引擎，容器退出。

## 常用其他命令

### 后台运行

启动后会发现该容器并没有运行。

因为想centos这种容器，如果在后台启动的话并不会给前台提供服务，所以docker自动就把他杀掉了。

但像mysql和Tomcat这种就不会有这样的问题

```bash
# -d 命令后台启动
[root@localhost ~]# docker run -d centos
9a19d1b6463a90a80aa0d76cc6ad0e502cd757133b6d319c00fc680c444c7aaf
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@localhost ~]# 

```

### 查看容器中的进程信息

```bash
[root@localhost ~]# docker top a05fc6ce10ae
UID          PID            PPID           C                   STIME               TTY                 TIME   			 CMD
root         10900          10879          0                   17:01               pts/0            00:00:00         /bin/bash
```

### 查看容器的元数据

```bash
[root@localhost ~]# docker inspect a05fc6ce10ae
[
    {
        "Id": "a05fc6ce10aed99d92a2cdeff574d3c4c25f075b5ddf19b251cafb1e9b9ec335",
        "Created": "2021-05-17T09:01:00.617175065Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 10900,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-05-17T09:01:01.103159533Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/a05fc6ce10aed99d92a2cdeff574d3c4c25f075b5ddf19b251cafb1e9b9ec335/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/a05fc6ce10aed99d92a2cdeff574d3c4c25f075b5ddf19b251cafb1e9b9ec335/hostname",
        "HostsPath": "/var/lib/docker/containers/a05fc6ce10aed99d92a2cdeff574d3c4c25f075b5ddf19b251cafb1e9b9ec335/hosts",
        "LogPath": "/var/lib/docker/containers/a05fc6ce10aed99d92a2cdeff574d3c4c25f075b5ddf19b251cafb1e9b9ec335/a05fc6ce10aed99d92a2cdeff574d3c4c25f075b5ddf19b251cafb1e9b9ec335-json.log",
        "Name": "/priceless_shannon",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/da94bb1742e3bde090b7944aebc8f6e2a344d9473c588b476e69dddf8246e27e-init/diff:/var/lib/docker/overlay2/761d8b173ce73ada55ef0894937d71ae33d76d657f8cc43791945f48a3cbc9ca/diff",
                "MergedDir": "/var/lib/docker/overlay2/da94bb1742e3bde090b7944aebc8f6e2a344d9473c588b476e69dddf8246e27e/merged",
                "UpperDir": "/var/lib/docker/overlay2/da94bb1742e3bde090b7944aebc8f6e2a344d9473c588b476e69dddf8246e27e/diff",
                "WorkDir": "/var/lib/docker/overlay2/da94bb1742e3bde090b7944aebc8f6e2a344d9473c588b476e69dddf8246e27e/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "a05fc6ce10ae",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201204",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "7cf59d99aaa9d53cb0b2743e8b186bc7bfec9dfcf52108804cd33eb75332bb79",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/7cf59d99aaa9",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "4111005c80a93f415c1219bcb18c4606f833225ee7b957de37122f8a0c7d2f7d",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.3",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:03",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "6ab4b1bbc67047ef1579b7fe6a641db04579251fe22c4cd46de2bed756eeb64e",
                    "EndpointID": "4111005c80a93f415c1219bcb18c4606f833225ee7b957de37122f8a0c7d2f7d",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:03",
                    "DriverOpts": null
                }
            }
        }
    }
]

```

### 进入当前正在运行的容器

`docker exec：`

Run a command in a running container,翻译过来就是在一个正在运行的容器中执行命令，exec是针对已运行的容器实例进行操作，在已运行的容器中执行命令，不创建和启动新的容器，退出shell不会导致容器停止运行。

`docker attach：`

Attach local standard input, output, and error streams to a running container，翻译过来，将本机的标准输入（键盘）、标准输出（屏幕）、错误输出（屏幕）附加到一个运行的容器，也就是说本机的输入直接输到容器中，容器的输出会直接显示在本机的屏幕上，如果退出容器的shell，容器会停止运行。

总结：

如果我们使用`docker attach`，我们只能使用一个shell实例。

所以如果我们想用容器的shell的新实例打开新的终端，我们需要运行`docker exec` 

如果Docker容器是使用`/bin/bash`命令启动的，则可以使用attach来访问它，如果不是，则需要在容器内创建一个bash实例。

attach 不会在容器中创建进程执行额外的命令，只是附着到容器上.

exec会在运行的容器上创建进程执行新的命令。

```bash
docker exec -it 容器id bashshell
docker attach 容器id
```

### 从容器内拷贝文件到主机上

```bash
docker cp 容器id:容器内路径 目的主机路径  
```

### 小结

![image-20210517181439735](https://gitee.com/mw515031/image/raw/master/image/image-20210517181439735.png)

# 可视化

## portainer

Docker的图形化界面管理工具  

```bash
docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

启动后即可根据`ip:8088`访问该工具。第一次登陆需要设置用户名和密码。

![image-20210517201107272](https://gitee.com/mw515031/image/raw/master/image/image-20210517201107272.png)

成功登陆后即可进入管理页面。

![image-20210517201241098](https://gitee.com/mw515031/image/raw/master/image/image-20210517201241098.png)

# Docker镜像

## 镜像

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。


一、UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

## 镜像加载原理

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

**bootfs(boot file system)**主要包含bootloader和kernel, bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

**rootfs (root file system)** 在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。 

## 分层原理

所有的 Docker镜像都起始于一个基础镜像层，当进行修改或培加新的内容时，就会在当前镜像层之上，创建新的镜像层。

举一个简单的例子，假如基于 Ubuntu Linux16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创建第三个镜像层。

该镜像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

![image-20210517203543920](https://gitee.com/mw515031/image/raw/master/image/image-20210517203543920.png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而整体的大镜像包含了来自两个镜像层的6个文件。

![image-20210517203625496](https://gitee.com/mw515031/image/raw/master/image/image-20210517203625496.png)

上图中的镜像层跟之前图中的略有区別，主要目的是便于展示文件。

下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版。

![image-20210517203726558](https://gitee.com/mw515031/image/raw/master/image/image-20210517203726558.png)

这种情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中。

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统。

Linux上可用的存储引撃有AUFS、 Overlay2、 Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于 Linux中对应的文件系统或者块设备技术，井且每种存储引擎都有其独有的性能特点。

Docker在 Windows上仅支持 windowsfilter 一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW [1]。

**特点：**

Docker 镜像都是只读的，当容器启动时，一个新的可写层加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！

##  创建一个镜像

我们从镜像中实例化出一个容器，我们可以在这个容器上进行自己的定制，然后进行打包，生成属于自己的镜像。

```bash
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="描述信息" -a="作者" 容器id 目标镜像名:[版本TAG]
```

# Docker数据卷

## 什么是容器数据卷

**docker的理念回顾**

将应用和环境打包成一个镜像！

数据？如果数据都在容器中，那么我们容器删除，数据就会丢失！需求：数据可以持久化

MySQL，容器删除了，删库跑路！需求：MySQL数据可以存储在本地！

容器之间可以有一个数据共享的技术！Docker容器中产生的数据，同步到本地！

这就是卷技术！目录的挂载，将我们容器内的目录，挂载到Linux上面！

![image-20210518122654125](https://gitee.com/mw515031/image/raw/master/image/image-20210518122654125.png)

**总结一句话：容器的持久化和同步操作！容器间也是可以数据共享的！**

## 使用数据卷

- `docker run -v name:/容器路径`：把容器的路径同步出来
- `docker run -v /宿主机路径:/容器路径`：把宿主机目录同步进去

### 一、直接挂载到宿主机

==这种方式比较适合把宿主机的目录同步到容器中。因为会覆盖容器对应路径的内容。所以建议挂载到容器内的空目录==

```bash
docker run -it -v 主机目录:容器目录

# 测试
[root@localhost home]# docker run -it -v /home/ceshi:/home  centos  /bin/bash
```

**显示文件的挂载信息**  

启动起来的时候，我们可以通过`docker inspect 容器id` 来查看挂载情况：（见下图）

![image-20210518123034091](https://gitee.com/mw515031/image/raw/master/image/image-20210518123034091.png)

在容器内指定目录下添加或修改一个文件，会同步到主机指定目录下！反之，在主机目录下做相关操作，也会同步到容器对应的目录下！

**查看所有的volume**

```bash
# 这里只会显示具名挂载和匿名挂载的信息（即下节的三、四），而直接指定本机路径的挂载，在此处不会显示
[root@localhost ~]# docker volume ls
DRIVER    VOLUME NAME
local     d73e0a32493e449b703ac6c9b51e45c66050619e512cb4194008ba6d0d654b82
```

### 直接挂载的几种情况

Docker容器启动的时候，如果要挂载宿主机的一个目录，可以用-v参数指定。

譬如我要启动一个centos容器，宿主机的/test目录挂载到容器的/soft目录，可通过以下方式指定：

\# docker run -it -v /test:/soft centos /bin/bash

这样在容器启动后，容器内会自动创建/soft的目录。通过这种方式，我们可以明确一点，即-v参数中，==冒号":"前面的目录是宿主机目录，后面的目录是容器内目录。==

==如果挂载到容器中的一个非空目录，那么容器中该目录的原始存在的文件会被清空==

貌似简单，其实不然，下面我们来验证一下：

**一、容器目录不可以为相对路径**

```bash
[root@localhost ~]# docker run -it -v /test:soft centos /bin/bash
invalid value "/test:soft" for flag -v: soft is not an absolute path
See 'docker run --help'.
```

直接报错，提示soft不是一个绝对路径，所谓的绝对路径，必须以下斜线“/”开头。

**二、宿主机目录如果不存在，则会自动生成**

如果宿主机中存在/test目录，首先删除它

```
[root@localhost ~]# rm -rf /test
[root@localhost ~]# ls /
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

启动容器

```
[root@localhost ~]# docker run -it -v /test:/soft centos /bin/bash
[root@a487a3ca7997 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  soft  srv  sys  tmp  usr  var
```

查看宿主机，发现新增了一个/test目录

```
[root@localhost ~]# ls /
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  test  tmp  usr  var
```

**三、宿主机的目录如果为相对路径呢？**

这次，我们换个目录名test1试试

```bash
# docker run -it -v test1:/soft centos /bin/bash
```

再到宿主机上查看是否新增了一个/test1目录，结果没有，是不是因为我用的是相对路径，所以生成的test1目录在当前目录下，结果发现还是没有。那容器内的/soft目录挂载到哪里去了？通过docker inspect命令，查看容器“Mounts”那一部分，我们可以得到这个问题的答案。

```json
"Mounts": [
    {
        "Name": "test1",
        "Source": "/var/lib/docker/volumes/test1/_data",
        "Destination": "/soft",
        "Driver": "local",
        "Mode": "z",
        "RW": true
    }
],
```

可以看出，容器内的/soft目录挂载的是宿主机上的/var/lib/docker/volumes/test1/_data目录

原来，所谓的相对路径指的是/var/lib/docker/volumes/，与宿主机的当前目录无关。

==与下边这种情况相比，相当于起了个名字。==

==这种方式适用于把容器内的目录同步到宿主机上。直观上可以理解为在`/var/lib/docker/volumes/`上创建了个目录，并把容器的目录同步过来==

**四、如果只是-v指定一个目录，这个又是如何对应呢？**

启动一个容器

```bash
[root@localhost ~]# docker run -it -v /test2 centos /bin/bash
[root@ea24067bc902 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  test2  tmp  usr  var
```

同样使用docker inspect命令查看宿主机的挂载目录

```json
   "Mounts": [
        {
            "Name": "96256232eb74edb139d652746f0fe426e57fbacdf73376963e3acdb411b3d73a",
            "Source": "/var/lib/docker/volumes/96256232eb74edb139d652746f0fe426e57fbacdf73376963e3acdb411b3d73a/_data",
            "Destination": "/test2",
            "Driver": "local",
            "Mode": "",
            "RW": true
        }
    ],
```

可以看出，同3中的结果类似，只不过，它不是相对路径的目录名，而是随机生成的一个目录名。

**五、如果在容器内修改了目录的属主和属组，那么对应的挂载点是否会修改呢？**

首先开启一个容器，查看容器内/soft目录的属性

```bash
[root@localhost ~]# docker run -it -v /test:/soft centos /bin/bash
[root@b5ed8216401f /]# ll -d /soft/
drwxr-xr-x 2 root root 6 Sep 24 03:48 /soft/
```

查看宿主机内/test目录的属性

```bash
[root@localhost ~]# ll -d /test/
drwxr-xr-x 2 root root 6 Sep 24 11:48 /test/
```

在容器内新建用户，修改/soft的属主和属组

```bash
[root@b5ed8216401f /]# useradd victor
[root@b5ed8216401f /]# chown -R victor.victor /soft/
[root@b5ed8216401f /]# ll -d /soft/
drwxr-xr-x 2 victor victor 6 Sep 24 03:48 /soft/
```

再来看看宿主机内/test目录的属主和属组是否会发生变化？

```bash
[root@localhost ~]# ll -d /test/
drwxr-xr-x 2 mycat mycat 6 Sep 24 11:48 /test/
```

竟然变为mycat了。。。

原来，这个与UID有关系，UID，即“用户标识号”，是一个整数，系统内部用它来标识用户。一般情况下它与用户名是一一对应的。

首先查看容器内victor对应的UID是多少，

```bash
[root@b5ed8216401f /]# cat /etc/passwd | grep victor
victor:x:1000:1000::/home/victor:/bin/bash
```

victor的UID为1000，那么宿主机内1000对应的用户是谁呢？

```bash
[root@localhost ~]# cat /etc/passwd |grep 1000
mycat:x:1000:1000::/home/mycat:/bin/bash
```

可以看出，宿主机内UID 1000对应的用户是mycat。

**六、容器销毁了，在宿主机上新建的挂载目录是否会消失？**

在这里，主要验证两种情况：一、指定了宿主机目录，即 -v /test:/soft。二、没有指定宿主机目录，即-v /soft

**第一种情况：**

```bash
[root@localhost ~]# rm -rf /test    --首先删除宿主机的/test目录
[root@localhost ~]# ls /    --可以看到，宿主机上无/test目录
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@localhost ~]# docker run -it --name=centos_test -v /test:/soft centos /bin/bash  --启动容器，为了删除方便，我用--name参数指定了容器的名字
[root@82ad7f3a779a /]# exit
exit
[root@localhost ~]# docker rm centos_test   --删除容器
centos_test
[root@localhost ~]# ls /   --发现 /test目录依旧存在
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  test  tmp  usr  var
```

可以看出，即便容器销毁了，新建的挂载目录不会消失。进一步也可验证，如果宿主机目录的属主和属组发生了变化，容器销毁后，宿主机目录的属主和属组不会恢复到挂载之前的状态。

**第二种情况：**

通过上面的验证知道，如果没有指定宿主机的目录，则容器会在/var/lib/docker/volumes/随机配置一个目录，那么我们看看这种情况下的容器销毁是否会导致相应目录的删除

首先启动容器

```bash
[root@localhost ~]# docker run -it --name=centos_test -v /soft centos /bin/bash
[root@6b75579ec934 /]# exit
exit
```

通过docker inspect命令查看容器在宿主机上生成的挂载目录

```json
    "Mounts": [
        {
            "Name": "b53164cb1c9f1917788638692fb22ad11994cf1fbbc2461b6c390cd3e10ea301",
            "Source": "/var/lib/docker/volumes/b53164cb1c9f1917788638692fb22ad11994cf1fbbc2461b6c390cd3e10ea301/_data",
            "Destination": "/soft",
            "Driver": "local",
            "Mode": "",
            "RW": true
        }
    ],
```

对应的是/var/lib/docker/volumes/b53164cb1c9f1917788638692fb22ad11994cf1fbbc2461b6c390cd3e10ea301/_data目录

销毁容器，看目录是否存在

```bash
[root@localhost ~]# docker rm centos_test
centos_test
[root@localhost ~]# ll /var/lib/docker/volumes/b53164cb1c9f1917788638692fb22ad11994cf1fbbc2461b6c390cd3e10ea301
total 0
drwxr-xr-x 2 root root 6 Sep 24 14:25 _data
```

发现该目录依旧存在，即便重启了docker服务，该目录依旧存在

```bash
[root@localhost ~]# systemctl restart docker
[root@localhost ~]# ll /var/lib/docker/volumes/b53164cb1c9f1917788638692fb22ad11994cf1fbbc2461b6c390cd3e10ea301
total 0
drwxr-xr-x 2 root root 6 Sep 24 14:25 _data
```

**拓展：**

```bash
# 通过 -v 容器内路径：ro 或 rw   改变容器内的目录权限
ro	 #readonly 只读
rw	 #readwrite 可读可写

# 一旦创建容器时设置了容器权限，容器对我们挂载出来的内容就有限定了！
docker run -d -P --name nginx05 -v juming:/etc/nginx:ro nginx
docker run -d -P --name nginx05 -v juming:/etc/nginx:rw nginx

# 默认是 rw
# ro 说明同步后容器只保存该路径读的权限。
```

**七、挂载宿主机已存在目录后，在容器内对其进行操作，报“Permission denied”。**

可通过两种方式解决：

1> 关闭selinux。

临时关闭：# setenforce 0

永久关闭：修改/etc/sysconfig/selinux文件，将SELINUX的值设置为disabled。

2> 以特权方式启动容器 

指定--privileged参数

如：# docker run -it --privileged=true -v /test:/soft centos /bin/bash

### 二、数据卷容器

如果我们不光要实现容器到宿主机之间的数据同步，而且要实现容器之间的同步，那么就要用到数据卷容器。

数据卷容器的思想：

把一个容器当成数据卷容器，其他容器挂载在这个数据卷容器上，来把所有数据卷容器上的数据卷同步在自己的容器中。

```bash
docker run --name 本容器名 --volumes-from 数据卷容器名 镜像名
```

实现分析：

上一节讨论了容器到宿主机的数据同步，要想实现多容器之间的数据同步，那么很直观就能想到：==把多个容器都和宿主机进行同步即可。==

所以在使用`--volumes-from`指定数据卷容器时，看似是子容器向父容器建立继承关系，但是我们可以通过`docker inspect 容器id`可以查到，两个容器其实只是绑定在宿主机上的同一个目录上了。所以这里所谓的继承只是==从父容器中读取数据卷，并配置到自己的容器中==。

当我们删除了本地目录后，就无法进行同步。

```bash
[root@b3d651f9dd81 home]# touch file1
touch: cannot touch 'file1': No such file or directory
```

# DockerFile

Dockerfile 是一个用来构建镜像的文本文件。一个dockerfile就标识了一个确切的镜像。只要所有人通过相同的Dockerfile生成镜像，就能保证所有人的环境统一。一个Dockerfile就唯一确定了一个镜像。

## 构建镜像

在 Dockerfile 文件的存放目录下，执行构建动作。

以下示例，通过目录下的 Dockerfile 构建一个 nginx:v3（镜像名称:镜像标签）。

```bash
$ docker build -f dockerfilename -t nginx:v3 .
```

命令最后的`.`代表上下文路径，是指 docker 在构建镜像，有时候想要使用到本机的文件（比如复制），docker build 命令得知这个路径后，会将路径下的所有内容打包。

**解析**：由于 docker 的运行模式是 C/S。我们本机是 C，docker 引擎是 S。实际的构建过程是在 docker 引擎下完成的，所以这个时候无法用到我们本机的文件。这就需要把我们本机的指定目录下的文件一起打包提供给 docker 引擎使用。

如果未说明最后一个参数，那么默认上下文路径就是 Dockerfile 所在的位置。

**注意**：上下文路径下不要放无用的文件，因为会一起打包发送给 docker 引擎，如果文件过多会造成过程缓慢。

## Docker命令

![image-20210518170448125](https://gitee.com/mw515031/image/raw/master/image/image-20210518170448125.png)

### From

定制的镜像都是基于 FROM 的镜像。

### RUN

用于执行后面跟着的命令行命令。有以下俩种格式：

shell 格式：

```dockerfile
RUN <命令行命令>
# <命令行命令> 等同于，在终端操作的 shell 命令。
```

exec 格式：

```dockerfile
RUN ["可执行文件", "参数1", "参数2"]
# 例如：
# RUN ["./test.php", "dev", "offline"] 等价于 RUN ./test.php dev offline
```

**注意**：Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大。例如：

```dockerfile
FROM centos
RUN yum install wget
RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz"
RUN tar -xvf redis.tar.gz
以上执行会创建 3 层镜像。可简化为以下格式：
FROM centos
RUN yum install wget \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
    && tar -xvf redis.tar.gz
```

如上，以 **&&** 符号连接命令，这样执行后，只会创建 1 层镜像。

### CMD

我们先来看运行一个容器的命令：

`docker run 镜像名 <命令>`。CMD就相当于<命令>这里的内容，如果指定了命令，就会替换CMD的内容。

类似于 RUN 指令，用于运行程序，但二者运行的时间点不同:

- CMD 在docker run 时运行。
- RUN 是在 docker build 时运行。

**作用**：为启动的容器指定默认要运行的程序，程序运行结束，容器也就结束。CMD 指令指定的程序可被 docker run 命令行参数中指定要运行的程序所覆盖。

**注意**：如果 Dockerfile 中如果存在多个 CMD 指令，仅最后一个生效。

格式：

```dockerfile
CMD <shell 命令> 
CMD ["<可执行文件或命令>","<param1>","<param2>",...] 
CMD ["<param1>","<param2>",...]  # 该写法是为 ENTRYPOINT 指令指定的程序提供默认参数
```

推荐使用第二种格式，执行过程比较明确。第一种格式实际上在运行的过程中也会自动转换成第二种格式运行，并且默认可执行文件是 sh。

### ENTRYPOINT

类似于 CMD 指令，但其不会被 docker run 的命令行参数指定的指令所覆盖，而且这些命令行参数会被当作参数送给 ENTRYPOINT 指令指定的程序。

但是, 如果运行 docker run 时使用了 --entrypoint 选项，将覆盖 ENTRYPOINT 指令指定的程序。

**优点**：在执行 docker run 的时候可以指定 ENTRYPOINT 运行所需的参数。

**注意**：如果 Dockerfile 中如果存在多个 ENTRYPOINT 指令，仅最后一 个生效。

格式：

```dockerfile
ENTRYPOINT ["<executeable>","<param1>","<param2>",...]
```

可以搭配 CMD 命令使用：一般是变参才会使用 CMD ，这里的 CMD 等于是在给 ENTRYPOINT 传参，以下示例会提到。

示例：

假设已通过 Dockerfile 构建了 nginx:test 镜像：

```dockerfile
FROM nginx

ENTRYPOINT ["nginx", "-c"] # 定参
CMD ["/etc/nginx/nginx.conf"] # 变参 
```

1、不传参运行

```dockerfile
$ docker run  nginx:test
```

容器内会默认运行以下命令，启动主进程。

```dockerfile
nginx -c /etc/nginx/nginx.conf
```

2、传参运行

```dockerfile
$ docker run  nginx:test -c /etc/nginx/new.conf -l xxx
```

容器内会默认运行以下命令，启动主进程(/etc/nginx/new.conf:假设容器内已有此文件)

```dockerfile
nginx -c /etc/nginx/new.conf -l xxx
```

### COPY

复制指令，从上下文目录中复制文件或者目录到容器里指定路径。

格式：

```dockerfile
COPY [--chown=<user>:<group>] <源路径1>...  <目标路径>
COPY [--chown=<user>:<group>] ["<源路径1>",...  "<目标路径>"]
```

**[--chown=<user>:<group>]**：可选参数，用户改变复制到容器内文件的拥有者和属组。

**<源路径>**：源文件或者源目录，这里可以是通配符表达式，其通配符规则要满足 Go 的 filepath.Match 规则。例如：

```dockerfile
COPY hom* /mydir/
COPY hom?.txt /mydir/
```

**<目标路径>**：容器内的指定路径，该路径不用事先建好，路径不存在的话，会自动创建。

```dockerfile
COPY source/ /destination	#把souorce目录下的所有文件（不包含source目录），拷贝到工作目录的destination目录下
COPY source destination		#把source文件保存到工作目录，重命名为destination
COPY /source destination	#把source目录复制到工作目录，重命名为destination
COPY /source /destination
```

**总结：**

- 原路径要是文件名，就代表拷贝该文件；原路径要是目录名，就代表拷贝该目录下的所有文件（不包括目录）
- 目的路径要是相对路径，就在工作目录（WORKDIR）里建立该目录
- 原路径是文件名，目标路径最后没有`/`，相当于把源文件重命名；有就放在该目录下

### ADD

ADD 指令和 COPY 的使用格式一致（同样需求下，官方推荐使用 COPY）。功能也类似，不同之处如下：

- ADD 的优点：在执行 <源文件> 为 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，会自动复制并解压到 <目标路径>。
- ADD 的缺点：在不解压的前提下，无法复制 tar 压缩文件。会令镜像构建缓存失效，从而可能会令镜像构建变得比较缓慢。具体是否使用，可以根据是否需要自动解压来决定。

### ENV

设置环境变量，定义了环境变量，那么在后续的指令中，就可以使用这个环境变量。容器运行时，这些环境变量依然有效。

格式：

```dockerfile
ENV <key> <value>
ENV <key1>=<value1> <key2>=<value2>...
```

以下示例设置 NODE_VERSION = 7.2.0 ， 在后续的指令中可以通过 $NODE_VERSION 引用：

```dockerfile
ENV NODE_VERSION 7.2.0

RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc"
```

### ARG

构建参数，与 ENV 作用一至。不过作用域不一样。ARG 设置的环境变量仅对 Dockerfile 内有效，也就是说只有 docker build 的过程中有效，构建好的镜像内不存在此环境变量。

构建命令 docker build 中可以用 --build-arg <参数名>=<值> 来覆盖。

格式：

```dockerfile
ARG <参数名>[=<默认值>]
```

### VOLUME

定义匿名数据卷。在启动容器时忘记挂载数据卷，会自动挂载到匿名卷。

作用：

- 避免重要的数据，因容器重启而丢失，这是非常致命的。
- 避免容器不断变大。

格式：

```dockerfile
VOLUME ["<路径1>", "<路径2>"...]
VOLUME <路径>
```

在启动容器 docker run 的时候，我们可以通过 -v 参数修改挂载点。

### EXPOSE

仅仅只是声明端口。

作用：

- 帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射。
- 在运行时使用随机端口映射时，也就是 docker run -P 时，会自动随机映射 EXPOSE 的端口。

格式：

```dockerfile
EXPOSE <端口1> [<端口2>...]
```

### WORKDIR

WORKDIR指令为Dockerfile中的任何 RUN、CMD、ENTRYPOINT、COPY 和 ADD指令设置工作目录。

指定工作目录。用 WORKDIR 指定的工作目录，会在构建镜像的每一层中都存在。（WORKDIR 指定的工作目录，必须是提前创建好的）。

docker build 构建镜像过程中的，每一个 RUN 命令都是新建的一层。只有通过 WORKDIR 创建的目录才会一直存在。

格式：

```dockerfile
WORKDIR <工作目录路径>
```

### USER

用于指定执行后续命令的用户和用户组，这边只是切换后续命令执行的用户（用户和用户组必须提前已经存在）。

格式：

```dockerfile
USER <用户名>[:<用户组>]
```

### HEALTHCHECK

用于指定某个程序或者指令来监控 docker 容器服务的运行状态。

格式：

```dockerfile
HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令
HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令

HEALTHCHECK [选项] CMD <命令> : 这边 CMD 后面跟随的命令使用，可以参考 CMD 的用法。
```

### ONBUILD

用于延迟构建命令的执行。简单的说，就是 Dockerfile 里用 ONBUILD 指定的命令，在本次构建镜像的过程中不会执行（假设镜像为 test-build）。当有新的 Dockerfile 使用了之前构建的镜像 FROM test-build ，这是执行新镜像的 Dockerfile 构建时候，会执行 test-build 的 Dockerfile 里的 ONBUILD 指定的命令。

格式：

```dockerfile
ONBUILD <其它指令>
```

## 小结

![image-20210519123947737](https://gitee.com/mw515031/image/raw/master/image/image-20210519123947737.png)

# Docker网络

## 原理

docker会在宿主机上虚拟出一个网卡，而借助这个网卡实现网络功能。

```bash
[root@localhost webapps]# ifconfig
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:4fff:feea:cca6  prefixlen 64  scopeid 0x20<link>
        ether 02:42:4f:ea:cc:a6  txqueuelen 0  (Ethernet)
        RX packets 163  bytes 212042 (207.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 215  bytes 22348 (21.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

我们如果把整个docker服务当成一个完整的局域网，那么这个`172.0.17.0.1`就是这整个网络的网关。这里可以比作手机热点。

当docker实例化一个容器后，会给这个容器分配一个ip地址。而这个IP地址在docker网络的网段中，这样整个docker网络就具备了通信的潜质。

```bash
[root@localhost webapps]# docker inspect tomcat01
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "7b9af3933449732939edc82b4211267428b0e30d9f4959368fecb88d7c3fe58a",
                    "EndpointID": "138f264772d2c8697fbf6f114da7957e9b3be1ec255f451ae676eda8fbd0a68e",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }

# 如果这里在实例化一个容器，会分配另一个地址
[root@localhost webapps]# docker inspect tomcat02
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "7b9af3933449732939edc82b4211267428b0e30d9f4959368fecb88d7c3fe58a",
                    "EndpointID": "24385d73d2957e955b7381ef85bd1211f8c79c0f031a354f5975354cf3ecd11a",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:03",
                    "DriverOpts": null
                }
```

那docker实现容器内的通信就是一件水到渠成的事了。因为docker内的容器们都在一个局域网内，还有docker0这个网卡作为网关，因此他们能够互相通信。

那么外界是怎么通过宿主机和docker内的容器通信的呢？

我们先初始化一个tomcat容器，然后看看宿主机的NAT表

```bash
[root@localhost home]# docker run -itd --name tomcat02 -p 8080:8080 tomcat


[root@localhost home]# docker ps
CONTAINER ID   IMAGE     COMMAND             CREATED          STATUS          PORTS                                  NAMES
1ccea2196f2c   tomcat  "catalina.sh run" 8 seconds ago  Up 6 seconds    0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   tomcat02


[root@localhost home]# iptables -t nat -vnL|grep :8080
    0     0 MASQUERADE  tcp  --  *      *       172.17.0.3           172.17.0.3           tcp dpt:8080
    0     0 DNAT       tcp  --  !docker0 *       0.0.0.0/0            0.0.0.0/0            tcp dpt:8080 to:172.17.0.3:8080

```

我们发现容器和宿主机的映射，其实就配置在宿主机的NAT表里，这里显示把宿主机的8080端口映射到容器172.17.0.3的8080端口上了，而这个地址就是容器tomcat02的地址

**小结**

![image-20210519180109868](https://gitee.com/mw515031/image/raw/master/image/image-20210519180109868.png)

## 自定义网络

上一节讲的是docker默认的网络。我们也可以根据自己的需求配置自己的docker网络（如子网、ip网段，网关……）

并在实例化容器时可以使用`-–net 网络名`和`-ip ip地址`分配到对应的网络中

- `network`实现网络管理

```bash
[root@localhost webapps]# docker network --help

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

Run 'docker network COMMAND --help' for more information on a command.
```

- 创建网络


```bash
[root@localhost webapps]# docker network create --help

Usage:  docker network create [OPTIONS] NETWORK

Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by Network driver (default map[])
      --config-from string   The network from which to copy the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a network segment
```

## 网络连通

上一节学会了怎么创建一个网络，但是不同网络之间的容器肯定是不能互通的，因为不在同一个网段。但是如果要想实现不同网段容器的互通也是可以实现的。

```bash
docker network connect 网络名 容器名
```

这样的话，就像相当于把容器添加到了新的网络中去，这个容器就可以访问这个网络中的其他容器。

这时候这个容器同时处在两个网络中，也就是同时有了两个ip地址。（相当于加了一块网卡）

# Docker Compose

- 前面我们使用 Docker 的时候，定义 Dockerfile 文件，然后使用 docker build、docker run 等命令操作容器。然而微服务架构的应用系统一般包含若干个微服务，每个微服务一般都会部署多个实例，如果每个微服务都要手动启停，那么效率之低，维护量之大可想而知
- 使用 Docker Compose 可以轻松、高效的管理容器，它是一个用于定义和运行多容器 Docker 的应用程序工具

## 安装

国内地址安装

```bash
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

#赋权
chmod +x /usr/local/bin/docker-compose

#查看版本
docker-compose version
```

## 快速开始

https://docs.docker.com/compose/gettingstarted/

**1. 为项目设定目录**

```
mkdir composetest
cd composetest
```

**2. 在项目目录中创建一个名为`app.py`的文件**

```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```

In this example, `redis` is the hostname of the redis container on the application’s network. 

为什么这里不用指定ip地址就能找到该服务？猜想如下：

- 可能这里指的是服务名（第五步中配置）
- 设置别名

```json
"Networks": {
                "composetest_default": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "4897ed755508",
                        "redis"			# 容器id的别名为redis
                    ],
                    "NetworkID": "d026ec59a1760db635be81ee892b5185f866910f4bdf27d0e17ef36b3bec1750",
                    "EndpointID": "6b55b9cbfb50d87c5ca92f54bf235d371211f81935ee563cfdb1a01656b17148",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:02",

```

**3. 在项目中创建一个名为`requirements.txt`的文件**

方便`pip install`

```
flask
redis
```

**4. 创建一个Dockerfile**

python环境+刚才写的app.py

```dockerfile
# syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```

**5. 在`docker-compose.yml`定义服务**

- 该web服务使用从Dockerfile当前目录中构建的映像。然后，它将容器和主机绑定到暴露的端口5000。此示例服务使用Flask Web服务器的默认端口5000。
- 该redis服务使用 从Docker Hub注册表中提取的公共Redis映像。

```yml
version: "3.8"
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```

**6. 运行**

```bash
docker-compose up

# ------------

Creating composetest_redis_1 ... done
Creating composetest_web_1   ... done
Attaching to composetest_redis_1, composetest_web_1
redis_1  | 1:C 20 May 2021 07:26:04.237 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis_1  | 1:C 20 May 2021 07:26:04.237 # Redis version=6.2.3, bits=64, commit=00000000, modified=0, pid=1, just started
redis_1  | 1:C 20 May 2021 07:26:04.237 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis_1  | 1:M 20 May 2021 07:26:04.239 * monotonic clock: POSIX clock_gettime
redis_1  | 1:M 20 May 2021 07:26:04.241 * Running mode=standalone, port=6379.
redis_1  | 1:M 20 May 2021 07:26:04.242 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
redis_1  | 1:M 20 May 2021 07:26:04.242 # Server initialized
redis_1  | 1:M 20 May 2021 07:26:04.242 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
redis_1  | 1:M 20 May 2021 07:26:04.242 * Ready to accept connections
web_1    |  * Serving Flask app 'app.py' (lazy loading)
web_1    |  * Environment: production
web_1    |    WARNING: This is a development server. Do not use it in a production deployment.
web_1    |    Use a production WSGI server instead.
web_1    |  * Debug mode: off
web_1    |  * Running on all addresses.
web_1    |    WARNING: This is a development server. Do not use it in a production deployment.
web_1    |  * Running on http://172.18.0.3:5000/ (Press CTRL+C to quit)
```

**7. 测试**

![image-20210520153735353](https://gitee.com/mw515031/image/raw/master/image/image-20210520153735353.png)

## docker-compose.yml

https://docs.docker.com/compose/compose-file/compose-file-v3/#service-configuration-reference

官方提供的示例：

```yml
version: "3"
services:

  redis:
    image: redis:alpine
    ports:
      - "6379"
    networks:
      - frontend
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        constraints: [node.role == manager]

  vote:
    image: dockersamples/examplevotingapp_vote:before
    ports:
      - 5000:80
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure

  result:
    image: dockersamples/examplevotingapp_result:before
    ports:
      - 5001:80
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  worker:
    image: dockersamples/examplevotingapp_worker
    networks:
      - frontend
      - backend
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.role == manager]

  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]

networks:
  frontend:
  backend:

volumes:
  db-data:

```

![image-20210520192933669](https://gitee.com/mw515031/image/raw/master/image/image-20210520192933669.png)

# Docker Swarm













