本地MySQL与R语言连接
================

## MySQL安装

MySQL安装很简单，由于MySQL是开源的，我们直接去官网下载安装包，按照提示一步步点击next就可以安装成功了。为了防止出差错也可以参考一些论坛经验，如[Win10安装MySQL详细教程](https://blog.csdn.net/qq_42543312/article/details/81543753?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param)

Workbench的一些基本操作可以参考[MySQL
Workbench使用教程](http://c.biancheng.net/view/2625.html)

## R及RStudio安装

这两个安装更简单，先安装R，再安装RStudio。

## 建立本地连接

### 利用RMySQL连接

``` r
library(RMySQL)
conn <- dbConnect(MySQL(), dbname = "cnarea", username="root", 
                  password="密码", host="127.0.0.1", port=3306)
df <- dbReadTable(conn, "cnarea_2019")  # 读表
```

利用RMySQL连接处理中文时经常容易出现乱码的情况。

### 利用ODBC连接

ODBC连接需要进行简单的配置，见[R语言-连接MySQL数据库方法](https://www.cnblogs.com/awishfullyway/p/6668270.html)

``` r
library(RODBC)
channel <- odbcConnect("cnarea", uid="root", pwd="密码")
sqlTables(channel)
data <- sqlFetch(channel,"cnarea_2019")
```

总的来书本地的连接还是很简单的，看看博客论坛很快就能搞定。
