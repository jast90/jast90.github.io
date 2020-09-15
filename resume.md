---
layout: bootstrap
title: "简历-张知文"
permalink: /resume
---
### 联系方式

- Email: zhangz_w45@163.com
- 手机：MTMyNjU2NDIzNjc=（base64编码,[base64解码](https://tool.oschina.net/encrypt?type=3)）

### 个人信息

- 张知文/男/1991
- 本科/赣南师范大学/网络工程 2010-2014
- github: [https://github.com/jast90](https://github.com/jast90)

### 工作经历
#### 深圳友宝科斯科技有限公司（2016.11-2019.9） Java软件工程师
##### 职责
1. 负责自动售货机业务微服务平台建设
2. 参与项目的系统分析、设计，负责核心模块代码编写
3. 优化系统性能

#### 深圳前海第三方供应链数据方案有限公司（2016.4-2016.11）Java开发工程师
##### 职责
1. 参与项目的系统分析、设计，负责核心代码编写
3. 优化重构现有系统架构

#### 奕丰基金销售有限公司（2014.6-2016.3) Java开发工程师
##### 职责
1. 负责金融平台的建设
2. 参与项目系统分析、设计，任务分解，负责模块代码编写

### 项目经历

#### “智能售酒冰箱”微信小程序商城

##### 项目描述

该项目是一个卖酒小程序商城，C端项目，功能包括：商品列表、详情，附近的设备，购物车，结算、支付，我的订单，我的酒柜，礼品卡。

##### 实现技术

Spring MVC + Spring Cloud + Redis + MySQL + Kafka + maven

##### 职责描述

1. 负责项目搭建，商品列表、详情，结算、支付，我的酒柜等模块的开发。
2. 为了实现对于不同用户的价格定制，采用策略模式来查询商品价格。

#### 自动售货机运营平台（2017.11-2019.9）

##### 项目描述

该项目主要是对自助售货机运营管理。功能包括：系统账户权限，商品管理，设备管理，点位管理，设备商品管理，设备商品库存管理，设备商品价格管理，设备APP管理、广告管理，促销管理。平台包括：管理后台Web网站、运维端小程序、用户端微信公众号、微信小城序商城等子项目。项目按功能垂直划分模块，数据库是按模块进行分库。通过自建MQTT服务来实现与设备APP长连接。

##### 实现技术

Spring + SpringMVC + Freemarker + Mybatis + Spring Cloud + Kafka + Netty + Eclipse Paho Mqtt + Redis + MySQL + Ace Admin + jQuery  + Git + Jenkins + Nginx + maven
##### 职责描述

1. 负责管理后台Web网站设备APP、广告、设备商品价格、设备商品库存等模块的功能开发及迭代。
2. 负责运维端小程序服务端项目的搭建、管理、开发及维护。为了解决在线用户会话的管理，引入Redis会话实现方式，使项目重新部署后会话有效。为了支持多平台小程序接入，提取用户接入jar包，并采用工厂模式来提高可扩展性。为了实现设备关门后盘点，采用Kafka消息来进行盘点。为了将盘点结果显示在小程序页面上，引入MQTT.js实现。
3. 负责小程序商城项目的搭建、管理、部分模块的开发。为了实现按用户角色定制价格，采用策略模式来查询商品价格。
4. 通过重写SQL语句，优化设备商品价格查询，使查询速度从几秒提升到几百毫秒。
5. 通过设置Jenkins的丢弃旧构建策略来防止磁盘空间占用过多。

#### SaaS运营后台（2019.5～2019.9）

##### 项目描述

该项目主要是针对对视觉冰箱运营管理后台系统，采用前后端分离实现。

##### 实现技术

Spring + SpringMVC + Mybatis + Spring Cloud + Kafka + ElasticSearch + Redis + MySQL + OAuth2 JWT + Vue + Ant Design Pro + Git + Jenkins + maven

##### 职责描述

1. 负责库存管理模块的开发及需求整理
2. 接口文档编写及与前端对接


#### 星利源一站式订货平台-快消王商城（2016.4-2016.10）

##### 项目描述

快消王商城是一个手机Web端商城，用户是：便利店老板，为便利老板提供一站式的快消品进货，功能包括：按类目筛选商品，优选商品，购物车，下单，我的订单，我的返利。

##### 实现技术

Spring + Strut2/Spring MVC + Mybatis/Spring JDBC + MySQL + SVN/Git + ant + maven

##### 职责描述

1. 负责快消品电商商城功能迭代及维护。
2. 通过完善商城商品品类、下单返利增加用户下单量。根据门店POS机历史数据为门店生成推荐商品，帮助门店精准购买商品。在线兑奖功能完善快消品业务闭环。
3. 使用maven重构项目、引入Spring MVC
4. 完成了移动Web端商城、安卓端APP服务端接口开发以及接口文档编写。

#### 奕丰金融平台（2014.6-2016.3）

##### 项目描述

奕丰金融平台是奕丰的一个投资理财平台。本人参与了研究资讯，基金搜索、申购，投资组合，用户等模块开发。

##### 实现技术

Spring + Spring MVC + Spring Security + JSP + Apache Tiles + Hibernate + Oracle + SVN + maven

##### 职责描述

1. 负责基金销售系统功能开发与实现。
2. 完成了2项目，1个PC Web端，1个APP接口服务端
3. PC Web端主要完成研究资讯，基金搜索、申购，投资组合，用户模块。
4. APP服务端负责用户登入、研究资讯接口开发、文档编写以及与安卓、IOS端对接。
5. 结合已有app登入用户切换网络时登入状态失效等问题，提出了App服务端采用OAuth2认证机制重新架构。

### 技能清单

- 编程语言：熟练Java，Javascript
- 大数据：Hadoop/Hive
- Java框架：Spring/SpringMVC/Spring Security/Spring Boot/Spring Cloud/Mybatis/Jsoup
- 项目构建、版本管理、文档和部署工具：maven/Git/Swagger/Jenkins/Gitlab CI/DI
- 存储：MySQL/Redis/ElasticSearch
- 前端框架：Bootstrap/jQuery/React
- 服务器相关：Linux/Shell
- 性能分析：JConsole/VisualVM
