本地与服务器连接
================

## 本地MySQL与服务器MySQL连接

在Workbench中新建连接，按照提示输入公网地址，账户及密码就可以进行连接，很简单。这样就实现了不同用户获取数据进行分析了，最好给服务器上MySQL的用户设置一下权限，仅允许服务器上的RStudioServer进行数据存储。

## 本地R与服务器MySQL连接

这里要用外网地址

``` r
library(RMySQL)
con <- dbConnect(MySQL(), host="外网地址", dbname="test",
                 user="root", password="密码")

res <- dbSendQuery(con, "SELECT * FROM test_tb order by id DESC limit 1")
data <- dbFetch(res)
dbClearResult(res)

newdf <- data.frame(id = as.integer(tail(data$id, 1) + 1),
                    name = as.character(Sys.time()), stringsAsFactors = FALSE)
dbWriteTable(con, "test_tb", newdf, append = TRUE, row.names = FALSE)

dbDisconnect(con) # close
```

## 应用

### 每日3点收盘自动保持基金数据

设置[定时执行R脚本](https://blog.csdn.net/wzgl__wh/article/details/84963698)

*/1 * \* \* \* Rscript /home/xzz/LoveXzz/test\_append.R

下面为test\_append.R文件代码，注意这里要用内网地址。

``` r
library(RMySQL)
con <- dbConnect(MySQL(), host="内网地址", dbname="test",
                 user="root", password="密码")

res <- dbSendQuery(con, "SELECT * FROM test_tb order by id DESC limit 1")
data <- dbFetch(res)
dbClearResult(res)

newdf <- data.frame(id = as.integer(tail(data$id, 1) + 1),
                    name = as.character(Sys.time()), stringsAsFactors = FALSE)
dbWriteTable(con, "test_tb", newdf, append = TRUE, row.names = FALSE)

dbDisconnect(con) # close
```

完成了所有内容！
