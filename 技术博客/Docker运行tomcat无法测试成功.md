# Docker运行tomcat无法测试成功

使用docker运行tomcat容器时，并没有报任何错误。运行代码如下：

```bash
docker run -it --name tomcat01 -p 127.0.0.1:8080:8080 tomcat /bin/bash
```

这里进入bash是因为要tomcat的webapps下时空目录，所以要把webpaas.dist下的内容拷贝过去

端口也显示正常映射：

```bash
CONTAINER ID   IMAGE     COMMAND       CREATED             STATUS          PORTS                      NAMES
4ccc1516c848   tomcat    "/bin/bash"   About an hour ago   Up 52 minutes   127.0.0.1:8080->8080/tcp   tomcat
```

但是用curl命令测试时却报错：

```bash
[root@localhost bin]# curl 127.0.0.1:8080
curl: (56) Recv failure: Connection reset by peer
```

## 怀疑本地端口没开启

```bash
[root@localhost bin]# firewall-cmd --zone=public --query-port=8080/tcp
no

# 这里确实没开启，用以下命令开启
[root@localhost bin]# firewall-cmd --zone=public --add-port=8080/tcp --permanent
success

# 重启防火墙
systemctl restart firewalld.service

# 再次查看端口
[root@localhost bin]# firewall-cmd --zone=public --query-port=8080/tcp
yes
```

再次测试仍然报错

```
[root@localhost bin]# curl 127.0.0.1:8080
curl: (56) Recv failure: Connection reset by peer
```

# 解决

查看tomcat的dockfile最后一行

```dockerfile
# ........

EXPOSE 8080
CMD ["catalina.sh", "run"]
```

CMD是指docker run命令执行后，执行的命令。这里相当于启动完成docker后执行`catalina.sh run`命令，即启动tomcat服务。

不过如果在docker run执行时如果指定了命令，就会将CMD内的命令覆盖。

我们再看一下刚才启动docker的命令：

```bash
docker run -it --name tomcat01 -p 127.0.0.1:8080:8080 tomcat /bin/bash
```

这里的`/bin/bash`命令就覆盖了`catalina.sh run`，因此tomcat服务没有启动。

