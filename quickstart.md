# 快速开始



## 新建项目

* Bootstarp.properties中增加配置
* application.yml中增加配置
* Application.java 中增加注解 开启服务注册于发现 @EnableDiscoveryClient
* 开启Feign的远程调用注解 @EnableFeignClients

## ElasticSearch

> 查询速度快的关键倒排索引，先分词，例如红海行动：拆分成红海、行动，并且用数字记录下来，最终按照数字的匹配率进行排序

* 索引相当于数据库
* 类型相当于数据表

* 文档相当于数据
* 属性相当于列

可视化例如：mysql的客户端sqlyog，kibana

###  `安装`

sudo mkdir -p /mydata/elasticsearch/config

sudo mkdir -p /mydata/elasticsearch/data



## Docker命令：



* docker ps 正在运行的docker程序
* docker images 查看下载的docker镜像
* docker pull elasticsearch:7.4.2 下载7.4.2版本的elasticsearch
* 











