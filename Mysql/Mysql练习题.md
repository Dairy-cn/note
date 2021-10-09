## Mysql练习题



#### student表结构

| Field  | Type         |
| ------ | ------------ |
| name   | varchar(255) |
| course | varchar(255) |
| score  | int(11)      |

1.查询出至少两门成绩超过80的学生个数？

```mysql
select count(*) from (select count(*) from student where score>80 group by name having count(*)>1) as t
```

2.删除记录，只保留每个同学的最好科目成绩记录

```mysql
//查询出每个同学的姓名和最高成绩
select name,max(score) as score from student group by name
```

```mysql
//查询出最高分记录，最后用delete form t not in 删除
select s1.* from student s1 inner join (select name,max(score) as score from student group by name) s2
on s1.name=s2.name and s1.score=s2.score
```

3.查询出平均分超过80分的学生姓名和平均分

```mysql
select name,avg(score) from student group by name having avg(score)>80
```

4.查询出学生课程个数和总分

```mysql
select name,count(course),sum(score) from student group by name
```

5.查询出姓"张"的学生个数

```mysql
select count(name) from student  where name like '张%'
```

6.查询出各个学科的总成绩和平均分、人数

```mysql
select sum(score),avg(score),count(course) from student group by course
```

7.根据学科成绩排名，并显示排名

mysql8.0后有窗口函数，如rank(),row_number()等

```mysql
SELECT
	NAME,
	course,
	score,
	ROW_NUMBER () OVER (
		PARTITION BY course
		ORDER BY
			score DESC
	) rank
FROM
	student
```

8.0之前的版本可以考虑存储过程或定义变量方式来解决

