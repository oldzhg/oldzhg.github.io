title: Docker 在Mac上安装SQL Server 2017

date: 2019-06-13

tags: Tech

------

 一、镜像安装

1、从Docker库中拉取镜像

```bash
sudo docker pull microsoft/mssql-server-linux:2017-latest1
```

2、拉取完之后，运行该镜像

```bash
sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=1234qwer!' \
   -p 1401:1433 --name sql1 \
   -d microsoft/mssql-server-linux:2017-latest123
```

可以运行多个镜像，保证端口好不一样，name不一样 
`-p 1402:1434 --name sql2`

![这里写图片描述](//pic.oldzhg.com/uPic/S9duGP.jpg)

3、要查看您的Docker容器，请使用该docker ps命令。

```
sudo docker ps -a1
```

4、更改SA密码

```
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P '1234qwer!' \
   -Q 'ALTER LOGIN sa WITH PASSWORD="1234qwer!"'123
```

5、连接到SQL Server

1. 使用该docker exec -it命令在正在运行的容器中启动交互式bash shell。在以下示例sql1中–name，创建容器时由参数指定的名称。

```
 sudo docker exec -it sql1 "bash"1
```

1. 一旦进入容器，用sqlcmd本地连接。Sqlcmd默认不在路径中，因此您必须指定完整路径。

```
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P '1234qwer!'1
```

如果成功，您应该到达sqlcmd命令提示符：1>。

6、使用外部数据库连接工具来连接数据库 
![这里写图片描述](//pic.oldzhg.com/uPic/JZn2ww.jpg)

IP地址是电脑主机的地址，后面要跟`,1401`

### 二、还原数据库

1、创建镜像sql1的文件目录

```
sudo docker exec -it sql1 mkdir /var/opt/mssql/backup1
```

2、拷贝文件

> 可以将bak文件放到用户主目录下面

```
sudo docker cp CZSBGL3.bak sql1:/var/opt/mssql/backup1
```

3、还原数据库

```
// 还原bak文件
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost \
   -U SA -P '1234qwer!' \
   -Q 'RESTORE FILELISTONLY FROM DISK = "/var/opt/mssql/backup/CZSBGL3.bak"' \
   | tr -s ' ' | cut -d ' ' -f 1-212345
```

你应看到类似于下面的输出： 
![这里写图片描述](//pic.oldzhg.com/uPic/i7WRk0.png)

4、调用RESTORE DATABASE命令才能还原在容器内的数据库。 为每个文件上一步中指定新路径。

```
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P '1234qwer!' \
   -Q 'RESTORE DATABASE CZSBGL3 FROM DISK = "/var/opt/mssql/backup/CZSBGL3.bak" WITH MOVE "CZSBGL_NEW" TO "/var/opt/mssql/data/CZSBGL3.mdf", MOVE "CZSBGL_NEW_log" TO "/var/opt/mssql/data/CZSBGL3.ldf"'123
```

你应看到类似于下面的输出： 
![这里写图片描述](//pic.oldzhg.com/uPic/6ygzws.png)

再次连接数据库，你应该能看到数据库列表。