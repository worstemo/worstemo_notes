# 数据类型

### 布尔值 bool —— False 假、True 真

```python
bool():
字符串 => 布尔：
	1. 空字符串 => False
	2. 非空字符串 => True
整型 => 布尔
	3. 0 => False
	4. 非0 => True
```
>True 和任何值and

```python
and 与
or  或
not 非
in  属于
```

### 整型 int



### 字符串 str —— 不可变

```python
独有功能：
	1. 大小写：upper()、lower()
	2. 是否为数字：isdecimal()
	3. 是否以特定字符串为开头或结尾：startswith()、endswith()
	4. 替换指定字符串：replace()
	5. 去除字符串左右的特定字符（默认为空格）：
		strip()   左右都去除
		lstrip()  去除左边特定字符
		rstrip()  去除右边特定字符
	6. 填充字符串到指定长度：
		center()  字符串居中
		ljust()   字符串居左，右填充
		rjust()   字符串居右，左填充
		zfill()   左填充0
	7. 格式化输出：
		''.join()    以特定字符连接字符串
		''.format()  {}是占位符
	8. 以特定字符为界切分字符串：split()
```

```python
公共功能：
	1. 长度：len()
	2. 索引：str[index]
	3. 切片：str[start:end+1:step]
	4. for循环遍历字符串中的每个字符
```


### 列表 list

>列表，是一个有序且可变的容器，元素可以是多种不同的数据类型

```python
独有功能：
	1. 列表尾部添加元素：append()
	2. 特定索引前添加元素：insert()
	3. 删除第一个匹配到的元素：remove()
	4. 删除特定索引的元素（不写默认为尾部元素）：
		pop()
		关键字：del
	5. 清除列表元素：clear()
	6. 对列表元素进行排序：sort()
		默认从小到大
		若 reverse = True，则从大到小
```

```python
公共功能：
	1. 长度：len()
	2. 索引：list[index]
	3. 切片：list[start:end+1:step]
	4. for循环遍历列表中的每个元素
	5. 嵌套
```


### 元组 tuple

>元组，是一个有序且不可变的容器，元素可以是多种不同的数据类型
>
>1. 元组的元素个数不能修改
>
>2. 元组的元素也不能被替换成其他的值
>
>**注意：元组中的列表可以进行增删改元素的操作，即该列表自身内部的修改**

```python
独有功能：
	无
```

```python
公共功能：
	1. 长度：len()
	2. 索引：tuple[index]
	3. 切片：tuple[start:end+1:step]
	4. for循环遍历元组中的每个元素
	5. 嵌套
```

>**元组中只有1个元素时，该元素后要添加','**
>
>v1 = (1,)    v1是元组
>
>v2 = (1)     即 v2 = 1


### 字典 dict


