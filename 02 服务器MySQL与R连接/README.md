服务器MySQL与R连接
================

## 购买服务器

我选的阿里云轻量应用服务器，学生的话很便宜。这个服务器是Cent
OS系统，上面已经安装好了MySQL与WordPress，我自己又安装了R与RStudioServer。[轻量应用服务器](https://www.aliyun.com/product/swas?spm=5176.229592.h2v3icoap.16.79cd3d92QBEiwG)

## 建立服务器连接

先解释一下我为什么需要在服务器上建立这个连接。我的目的是每日收盘后获得各个基金的收盘行情，然后利用R语言存到MySQL中。放到服务器就可以设置crontab定时执行这个操作了。

### 设置服务器中的MySQL

这里要在防火墙里加上3306端口； 获取RmySQL的root账户的密码（阿里云官网讲的很清楚）。

### 设置RStudioServer

安装RMySQL包的支持程序，由于是Cent OS系统，所以需要执行：yum -y install mariadb-devel；
安装RmySQL包

### 连接

``` r
library(RMySQL)
con <- dbConnect(MySQL(), host="172.25.0.101", dbname="test",
                 user="root", password="密码")
summary(con)
dbListTables(con)

res <- dbSendQuery(con, "SELECT * FROM test_tb order by id DESC limit 1")
data <- dbFetch(res)
dbClearResult(res)

newdf <- data.frame(id = as.integer(tail(data$id, 1) + 1),
                    name = as.character(Sys.time()), stringsAsFactors = FALSE)

dbWriteTable(con, "test_tb", newdf, append = TRUE, row.names = FALSE)

dbDisconnect(con) # close
```

这里配置有点麻烦了。
