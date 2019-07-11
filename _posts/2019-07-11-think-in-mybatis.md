---
layout: bootstrap
title: 'Mybatis源码分析'
description: 'Think in Mybatis'
date: '2019-07-11 16:54:00'
author: Jast
---
# mybatis使用

## 创建SqlSessionFactory
### XML配置文件创建
### Java编码实现

## 创建SqlSession
### SqlSessionFactory.openSession()
#### 返回的是org.apache.ibatis.session.defaults.DefaultSqlSession

## 执行SQL
### 直接调用SqlSession方法执行
#### 用法
##### 要指定statement
##### statement是带包的类方法名
#### 实现
##### 具体的实现是在org.apache.ibatis.session.defaults.DefaultSqlSession中实现的
###### select的实现
- 1. 调用org.apache.ibatis.executor.Executor#query
- 2. defaultExecutorType是ExecutorType.SIMPLE
- 3. 调用org.apache.ibatis.executor.BaseExecutor#query
- 4. 先是查看缓存是否存在，存在直接返回，不存在调用org.apache.ibatis.executor.BaseExecutor#queryFromDatabase查数据库，继续往下执行
- 5. 查询数据库，org.apache.ibatis.executor.BaseExecutor#doQuery
- 6. 查询数据库，前面知道defaultExecutorType是ExecutorType.SIMPLE，调用org.apache.ibatis.executor.SimpleExecutor#doQuery
- 7. 创建org.apache.ibatis.executor.statement.StatementHandler，org.apache.ibatis.session.Configuration#newStatementHandler
- 8. 创建java.sql.Statement及设置参数，org.apache.ibatis.executor.SimpleExecutor#prepareStatement
- 9. 执行查询，org.apache.ibatis.executor.statement.StatementHandler#query
- 10. 执行JDBC查询及结果集处理并返回，org.apache.ibatis.executor.statement.PreparedStatementHandler#query

###### insert、update及delete的实现，三者的实现是一样的
- 1. 调用org.apache.ibatis.session.defaults.DefaultSqlSession#update
- 2. 调用org.apache.ibatis.executor.BaseExecutor#update
	- 2.1 调用org.apache.ibatis.executor.BaseExecutor#doUpdate
- 3. 调用org.apache.ibatis.executor.SimpleExecutor#doUpdate
	- 3.1 根据MappedStatement生成StatementHandler
	- 3.2 提供参数
- 4. 调用org.apache.ibatis.executor.statement.PreparedStatementHandler#update
	- 4.1 执行JDBC，调用java.sql.PreparedStatement#execute执行SQL语句
	- 4.2 获取影响的行数，调用java.sql.Statement#getUpdateCount
	- 4.3 获取主键生成器，调用org.apache.ibatis.mapping.MappedStatement#getKeyGenerator
	- 4.4 设置返回的主键值到主键属性，org.apache.ibatis.executor.keygen.KeyGenerator#processAfter
	- 4.5 返回影响的行数
### 获取Mapper执行
#### session.getMapper(）
- 1. 调用org.apache.ibatis.session.defaults.DefaultSqlSession#getMapper
- 2. 调用org.apache.ibatis.session.Configuration#getMapper
- 3. 调用org.apache.ibatis.binding.MapperRegistry#getMapper
- 4. 调用org.apache.ibatis.binding.MapperProxyFactory#newInstance
- 5. 调用java.lang.reflect.Proxy#newProxyInstance
#### 原理
##### Java 动态代理
#### 实现
- 1. 因为最终是使用动态代理创建的对象，所以我们只要关注InvocationHandler的实现，即：org.apache.ibatis.binding.MapperProxy
- 2. 调用org.apache.ibatis.binding.MapperMethod#execute
- 3. 根据MapperMethod对象确定sql语句类型，然后调用org.apache.ibatis.session.SqlSession的相关执行SQL语句的方法，即select*，insert，update，delete
- 4. 实际上调用的还是org.apache.ibatis.session.defaults.DefaultSqlSession中的相关实现，其实和直接调用SqlSession方法执行是一样的