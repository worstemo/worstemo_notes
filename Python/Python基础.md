# 数据类型

### 整型 int


### 字符串 str —— 不可变

```
私有方法：
	大小写：upper()、lower()
	是否为数字：isdecimal()
	是否以特定字符串为开头或结尾：startswith()、endswith()
	去除字符串左右的特定字符（默认为空格）：
		strip()   左右都去除
		lstrip()  去除左边特定字符
		rstrip()  去除右边特定字符
	填充字符串到指定长度：
		center()  字符串居中
		ljust()   字符串居左，右填充
		rjust()   字符串居右，左填充
		zfill()   左填充0
	格式化输出：
		''.join()    以特定字符连接字符串
		''.format()  {}是占位符
	以特定字符为界切分字符串：
		split()
```

```
公有方法：
	长度：len()
	索引：str[index]
	切片：str[start:end+1:step]
	
```