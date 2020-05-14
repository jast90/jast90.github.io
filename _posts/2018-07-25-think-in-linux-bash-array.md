---
layout: bootstrap
title: 'bash 数组 理解'
description: 'linux,bash array'
date: '2018-07-25 11:37:00'
author: Jast
---
### Bash数组  
1. 定义数组  
```
declare -a 数组名 #只定义数据不赋值
declare -a 数组名=(值1 值2)  #定义数组并赋值，多个值间用空格隔开，值包含空格时值用""或''
declare -a score=(90 91 92)
```

2. 数组赋值   
```
数组名[下标]=值
score[3]="Hello World"
```

3. 数组取值   
```
${数组名[下标]} # 获取数组单个元素
echo ${score[1]}
91
${数组名[@/*]} # 获取所有元素
echo ${score[*]}
90 91 92 hello world
echo ${score[@]}
90 91 92 hello world
```

4. 数组长度  
```
${#数组名[@/*]} #通过#来获取长度
echo ${#score[*]}
4
```

5. 数组截取   
```
${score[@/*]:起始下标:元素个数}		#获取数组中的的几个元素
echo ${score[@]:1:2}	# 获取score中的从下标为1开始的两个元素
91 92
${score[n]:元素的起始下标:字符个数 		#获取数组第n(从0开始)个元素的某几个字符
${score[3]:0:5} 	#获取score中下标为3的元素中的以第0个开始往后的5个字符
hello
```

6. 连接数组：将若干个数组进行拼接操作    
通过()加空格  
```
$ declare -a student=("jast" "test")
$ echo ${student[*]}
jast test
$ conn=(${score[*]} ${student[*]})
$ echo ${conn[*]}
90 91 92 hello world jast test
```

7. 替换元素  
通过数组赋值，然后通过${数组名[@]/匹配的字符串/替换成的字符串。  
```
$ declare -a strArray=("hello world" "hello jast" "hello bash")
$ echo ${strArray[@]}
hello world hello jast hello bash
$ strArray=(${strArray[@]/hello/hi)
$ echo ${strArray[@]}
hi world hi jast hi bash
```

8. 取消数组或元素  
取消数组  
```
$ declare -a unsetArray=("test" "asdad")
$ unset unsetArray[0]
$ echo ${unsetArray[0]}
-bash: unsetArray[0]: 未绑定的变量
$ unset unsetArray
$ echo ${unsetArray[1]}
-bash: unsetArray[1]: 未绑定的变量
```