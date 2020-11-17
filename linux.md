# Linux



## Linux常用命令

1、查看操作系统版本

```
$ lsb_release -a
```

``` 
$ lsb_release -cs
```

2、tee写入文件，方便复制

```
$ tee /etc/redis/redis.conf
```

3、查看主机名

``` shell
hosenamectl
```



## 配置IP地址

修改IP地址

``` shell
vi /etc/netplan/50.clound.init.yaml

netplan apply
```

