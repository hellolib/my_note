## 摘要

- **项目名称:** DataCompare

- **简介:** 数据比对 工作可视化

- 功能要点: golang, gin, database/sql, cron任务调度, vue

- **数据库:** mysql, postgres, vertica, oracle

- 容器: Docker

- 主题界面:

  <img src="https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220316162341809.png" alt="image-20220316162341809" style="zoom:50%;" />

## 功能特点

1. 支持**mysql, postgres, vertica, oracle** 数据库创建数据库连接
2. 支持配置源表和目标表信息
3. 可自行对数据比对任务进行调度
4. docker部署, 快速解决环境依赖问题
   - nginx 镜像
   - mysql5.7镜像
   - centos7.6镜像

## 环境依赖

1. docker 环境
2. oracle-cli 环境(仅在配置oracle连接时使用)

## 开发计划

- 测试任务创建时检查 config 和result 的表是否存在
- 任务详情 写入redis
- 自动补数 ...
- 结果页展示细化

## 使用说明

<img src="https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/image-20220316162219026.png" alt="image-20220316162219026" style="zoom:50%;" />