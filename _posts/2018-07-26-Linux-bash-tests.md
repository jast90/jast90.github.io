---
layout: bootstrap
title: 'bash 测试'
description: 'linux,bash test'
date: '2018-07-26 10:59:00'
author: Jast
---
## {{ page.title }} 
<i class="far fa-clock"></i>{{ page.date | date: "%Y-%m-%d"}}  <i class="far fa-user">{{ page.author }}  

### Bash 测试  
1. 使用方式  
```
#方式一
test 表达式
#方式二
[ 表达式 ] #表达式前后需要空格和[、]隔开
```
2. 文件测试  
2.1 linux文件类型  
- 普通文件（-）
- 目录（d）
- 字符设备文件（c）
- 块设备文件（b）
- 套接口文件（s）
- 符号链接文件（l）

参考[Linux文件类型全解析](http://os.51cto.com/art/201003/185612.htm)    

2.2 文件测试符  
```
-b 文件 		#文件存在且是块文件时返回true,否则返回false
-c 文件 		#文件存在且是字符文件时返回true,否则放回false
-d 文件 		#文件存在且是目录时返回true,否则放回false
-e 文件 		#文件或目录存在时返回true,否则放回false
-f 文件 		#文件存在且是普通文件时返回true,否则放回false
-x 文件 		#文件存在且是可执行文件时返回true,否则放回false
-w 文件 		#文件存在且是可写文件时返回true,否则放回false
-r 文件		 	#文件存在且是可读文件时返回true,否则放回false
-l 文件 		#文件存在且是链接文件时返回true,否则放回false
-p 文件 		#文件存在且是管道文件时返回true,否则放回false
-s 文件 		#文件存在且是大小不为0时返回true,否则放回false
-S 文件 		#文件存在且是socket文件时返回true,否则放回false
-g 文件 		#文件存在且设置了SGID时返回true,否则放回false
-u 文件 		#文件存在且设置了SUID时返回true,否则放回false
-k 文件 		#文件存在且设置了sticky属性时返回true,否则放回false
-G 文件 		#文件存在且属于有效的用户组时返回true,否则放回false
-O 文件 		#文件存在且属于有效的用户时返回true,否则放回false
文件1 -nt 文件2 #当文件1比文件2新时返回true,否则放回false。nt：是newer than的首字母缩写
文件1 -ot 文件2 #当文件1比文件2旧时返回true,否则放回false。ot：是older than的首字母缩写
```

3. 字符串测试
3.1 字符串测试符  
```
-z "string" 			# 字符串string为空时返回true，否则返回false
-n "string" 			# 字符串string非空时返回true，否则返回false
"string1"="string2" 	# 字符串string1和string2相同时返回true，否则返回false
"string1"!="string2" 	# 字符串string1和string2不相同时返回true，否则返回false
"string1">"string2" 	# 按照字典排序，字符串string1排在string2之前时返回true，否则返回false
"string1"<"string2"	 	# 按照字典排序，字符串string1排在string2之后时返回true，否则返回false
```

4. 整数比较  
4.1 整数测试符  
```
num1 -eq num2 # 如果num1等于num2时返回true，否则返回false。eq:equal 
num1 -gt num2 # 如果num1大于num2时返回true，否则返回false。gt:great than 
num1 -lt num2 # 如果num1小于num2时返回true，否则返回false。lq:less than 
num1 -ge num2 # 如果num1大于等于num2时返回true，否则返回false。ge:great than 
num1 -le num2 # 如果num1小于等于num2时返回true，否则返回false。le:less than
num1 -ne num2 # 如果num1不等于num2时返回true，否则返回false。ne:not equal 
```
5. 逻辑测试  
5.1 逻辑测试符  
```
! 表达式 			#逻辑非，取反运算，如果表达式为true，返回false,否则返回true 。
表达式1 -a 表达式2	#表达式1和表达式2同时为true时，返回true,否则返回false。a: and
表达式1 -o 表达式2	#表达式1和表达式2有一个为true时，返回true,否则返回false。a: or
```
以上符号应用在[]中，如：[-e /test -o -e /ad]  

5.2 逻辑运算符  
```
! 	# 逻辑非
&&	# 逻辑与
||	# 逻辑或
```
以上符号应用在多个[]之间，如：[-e /test] || [-e /ad]