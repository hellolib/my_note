<img src="https://raw.githubusercontent.com/hellolib/pictures/main/Typora/pic-00-gitee/20220726214526.png" alt="logo" style="zoom: 33%;" />

## 功能

### 1. awesome 模块

- 主要展示各语言的awesome开源库，可以快速找到你需要的开源库

- 支持语言awesome模块

  | 编程语言 | 是否支持 |
  | :------: | :------: |
  |  Python  |    ✅     |
  |    Go    |    ✅     |

- 数据来源

  - awesome-go：https://github.com/vinta/awesome-python

  - awesome-python: https://github.com/avelino/awesome-go

### 2. github 项目 top模块

> 数据来源：https://github.com/trending

- 功能分类

  1. github 星星排名 

     - 各语言 github 排名

     - 支持按语言查找

     - 数据来源: https://github.com/search?o=desc&q=stars%3A%3E50000&s=stars&type=Repositories

  2. 周榜：各语言 github 本周榜

     - 支持按语言查找
     - 数据来源: https://github.com/trending?since=weekly

  3. 月榜：各语言 github 本月榜

     - 支持按语言查找
     - 数据来源：https://github.com/trending?since=monthly

  4. 天榜：各语言 github 本天榜

     - 按语言查找
     - 数据来源： https://github.com/trending?since=daily

### 3. 编程语言排行榜模块

- 数据来源： https://www.tiobe.com/tiobe-index/

## 开发工作

### 1. 主页

- 狂拽炫酷的主页（前后端接口）

### 2. awesome 模块

- awesome 数据采集（python，go）
  - 后台数据库字段： 

- 数据接口
- 前台可视化页面

### 3. 编程语言排行榜模块

- 编程语言排行榜采集模块
- 数据接口
- 前台可视化页面

## 技术要点

### 1. 后端

1. go + gin
2. 爬虫（go）
3. postgres/mysql

### 2. 前端

1. vue

### 3. 部署

1. docker
2. nginx

## 开发进度跟踪

|      功能       | 计划完成日期 | 是否完成 |
| :-------------: | :----------: | :------: |
|  项目框架搭建   |  2022-07-26  |    ✅     |
| 数据库设计&搭建 |  2022-07-27  |    ✅     |
|  基础文档编写   |  2022-07-26  |    ✅     |
|   awesome-go    |  2022-07-27  |    ✅     |
| awesome-python  |  2022-07-27  |    ✅     |

