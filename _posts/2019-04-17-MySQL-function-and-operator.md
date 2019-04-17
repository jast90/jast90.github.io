---
layout: bootstrap
title: 'MySQL 方法和操作'
description: 'MySQL function and operator'
date: '2019-04-17 15:14:00'
author: Jast
---
## MySQL 方法和操作
### 参考
[MySQL 5.7 Function and Operator Reference](https://dev.mysql.com/doc/refman/5.7/en/func-op-summary-ref.html)

### 方法应用小结
#### FIND_IN_SET()
语法：FIND_IN_SET(str,strlist)  
功能：Index (position) of first argument within second argument：返回第一个参数在第二个参数中的索引，从1开始

#### BETWEEN ... AND ...
语法：expr BETWEEN min AND max
功能：Check whether a value is within a range of values（判断一个值是否在范围内，在返回1，不在返回0
）
#### GROUP_CONCAT()
语法：group_concat支持按字段连接、按字段排序，例如：`GROUP_CONCAT(a.exam_score_id ORDER BY a.`exam_score` DESC  SEPARATOR ",")`

```
GROUP_CONCAT([DISTINCT] expr [,expr ...]
             [ORDER BY {unsigned_integer | col_name | expr}
                 [ASC | DESC] [,col_name ...]]
             [SEPARATOR str_val])
```
功能：This function returns a string result with the concatenated non-NULL values from a group. It returns NULL if there are no non-NULL values. （返回按分组的、连接的非空字符串，如果不存在非NULL值返回NULL）

#### SUBSTRING_INDEX()  
语法：SUBSTRING_INDEX(str,delim,count)  
功能：Returns the substring from string str before count occurrences of the delimiter delim. If count is positive, everything to the left of the final delimiter (counting from the left) is returned. If count is negative, everything to the right of the final delimiter (counting from the right) is returned. 在分隔符delim的计数出现之前，从字符串str返回子字符串。如果count为正数，则返回最终分隔符左侧的所有内容（从左侧开始计算）。如果count为负数，则返回最终分隔符右侧的所有内容（从右侧开始计算）。

#### 实例
1. 查询分组下的前几名  
1.1 查询某场考试各班前三名成绩的学生信息  

表结构如下：
```
CREATE TABLE student(
s_id INT(11) NOT NULL PRIMARY KEY AUTO_INCREMENT,
s_name VARCHAR(64),
c_id INT(11) NOT NULL
);

CREATE TABLE exam_score(
exam_score_id INT(11) NOT NULL PRIMARY KEY AUTO_INCREMENT,
exam_id INT(11),
c_id INT(11) NOT NULL,
s_id INT(11) NOT NULL,
exam_score DECIMAL(13,2) NOT NULL
);
```

方案1

```
SELECT
  c.*,a.exam_score,a.exam_id
FROM
  (SELECT
    *
  FROM
    `exam_score` a,
    (SELECT
      SUBSTRING_INDEX(
        GROUP_CONCAT(
          a.`exam_score_id`
          ORDER BY a.`exam_score` DESC
        ),
        ",",
        3
      ) AS exam_score_ids
    FROM
      `exam_score` a
      LEFT JOIN `student` b
        ON a.`s_id` = b.`s_id`
    GROUP BY b.`c_id`) AS b
  WHERE FIND_IN_SET(
      a.`exam_score_id`,
      b.exam_score_ids
    )) AS a
  LEFT JOIN `student` c ON a.s_id = c.`s_id`
ORDER BY c.`c_id`,
  a.`exam_score` DESC
```

方案2

```
 SELECT
  a.*
FROM
  `exam_score` a,
  (SELECT
    GROUP_CONCAT(
      a.exam_score_id
      ORDER BY a.`exam_score` DESC SEPARATOR ","
    ) AS exam_score_ids,
    b.c_id AS c_id
  FROM
    `exam_score` a
    LEFT JOIN `student` b
      ON a.`s_id` = b.`s_id`
  GROUP BY b.`c_id`) AS b
WHERE FIND_IN_SET(
    a.`exam_score_id`,
    b.exam_score_ids
  ) BETWEEN 1
  AND 3
ORDER BY b.c_id ASC,
  a.`exam_score` DESC;
```