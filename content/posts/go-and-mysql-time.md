---
title: "Golang time.Time 和 MySQL datetime 互转问题"
date: 2023-11-05T16:10:00+08:00
draft: false
categories: [Golang]
tags: [MySQL]
---

## 一、时间概念
### [时间标准](https://en.wikipedia.org/wiki/Time_standard)
某个时间系统的时间标准又称时间基准，是定义如何测量时间的一种规范，具体包括测量时间间隔的尺度基准和定义起始时刻的参考基准。

UTC
- https://en.wikipedia.org/wiki/Coordinated_Universal_Time

### [标准时间](https://en.wikipedia.org/wiki/Standard_time)
标准时间是在同一时区内的不同地区，舍弃地区性的子午线定出的太阳时或地方平时，而共同采用的同步时间。

Time Zone
- https://en.wikipedia.org/wiki/Time_zone
- https://en.wikipedia.org/wiki/Tz_database
- https://en.wikipedia.org/wiki/List_of_tz_database_time_zones


### [国际标准 ISO 8610](https://en.wikipedia.org/wiki/ISO_8601)
国际标准化组织的日期和时间的表示方法。

Date
- 2023-11-05

Time in UTC
- 06:50:41Z
- T065041Z

Date and time in UTC
- 2023-11-05T06:50:41Z
- 20231105T065041Z


Date and time with the offset
- 2023-11-04T23:50:41−07:00 UTC−07:00
- 2023-11-05T06:50:41+00:00 UTC+00:00
- 2023-11-05T13:50:41+07:00 UTC+07:00

Ref:
- The T is just a literal to separate the date from the time, and the Z means "zero hour offset" also known as "Zulu time" (UTC).
- [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)
- [GMT](https://en.wikipedia.org/wiki/Greenwich_Mean_Time)
- [Time in China - CST](https://en.wikipedia.org/wiki/Time_in_China)
- [Time in the United States](https://en.wikipedia.org/wiki/Time_in_the_United_States)


## 二、Time in Linux
查看所在时区的日期和时间
- `date`
- `date -R`


### 设置系统所在时区
使用 /usr/share/zoneinfo 下时区文件替换系统时区文件
- `cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`
- `cp /usr/share/zoneinfo/America/Los_Angeles /etc/localtime`


### Docker alpine 容器下设置
Docker alpine 镜像下构建的容器没有时区文件，需要使用 apk 添加 tzdata

```
FROM alpine:latest

RUN apk add -U tzdata
ENV TZ=Asia/Shanghai
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```


## 三、Go time.Time 和 MySQL datetime 数据转换导致的时区混乱问题

一般需要保证下面三者所在时区一致才不会导致时间数据混乱

### MySQL 服务器时区
```
show variables like "%time_zone%";
```

### Go 服务所在服务器时区
```
date -R
```

### Go 服务读写 MySQL 所设置的 dsn location
parseTime 设为 true，则驱动会把 MySQL 的 DATETIME 类型和 Go 的 time.Time 互转。

```
parseTime=true&loc=Local
或具体时区
parseTime=true&loc=Asia%2FShanghai
```

PS：多个 MySQL 服务之间不在同一个时区，那么就需要统一时区，使用例如 loc=Asia%2FShanghai，不能再使用 loc=Local。

