## 数据类型

### 布尔值 bool

>False 假、True 真

```python
bool():
0、""、[]、()、{}、set()、None => False
其他 => True
```

**逻辑谓词：**

```python
and 与
or  或
not 非
in  属于
```

>**与：0与任何值结果为0，1与任何值结果为任何值**
>
>**或：1或任何值结果为1，0或任何值结果为任何值**

```python
v1 = 0 and 12       # v1 = 0
v2 = '1' and 99     # v2 = 99
v3 = '' or 18       # v3 = 18
v4 = 'hello' or ''  # v4 = 'hello'
```


### 整型 int

二进制 0b

```python
十进制 => 二进制字符串
bin()
```

八进制 0o

```python
十进制 => 八进制字符串
oct()
```

十六进制 0x

```python
十进制 => 十六进制字符串
hex()
```


### 浮点型 float

>浮点型，就是我们常说的小数
>
>注意：由于计算机底层浮点型的存储原理，有时候浮点型获取的值可能不会太精确

```python
# 精确的浮点数运算，可以使用 decimal模块
import decimal

v1 = decimal.Decimal('0.1')
v2 = decimal.Decimal('0.2')

v3 = v1 + v2
print(v3) # 0.3
```


### 字节类型 bytes

>**注意：UTF-8 中一个中文占3个字节，GBK 中一个中文占2个字节**

```python
字符串 => 字节 ：encode()
字节 => 字符串：decode()
```

```python
name = "酱油瓶"  # 字符串类型 str，底层采用 unicode 编码  
data1 = name.encode('utf-8')  # 字节类型 bytes，底层采用 utf-8 编码  
data2 = name.encode('gbk')  # 字节类型 bytes，底层采用 gbk 编码  
  
print(data1)  # b'\xe9\x85\xb1\xe6\xb2\xb9\xe7\x93\xb6'  
print(data2)  # b'\xbd\xb4\xd3\xcd\xc6\xbf'
```


### 字符串 str

>**注意：字符串是不可变的**

字符串编码

```python
unicode  # 字符串类型 str，底层采用 unicode 编码
utf-8    # 字节类型 bytes，底层采用 utf-8 编码
```

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
	5. in是否包含
```


### 列表 list

>列表，是一个**有序**且**可变**的容器，元素可以是多种不同的数据类型

```python
定义列表：
1. item = []
2. item = list()
```

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
	6. in是否包含
```


### 元组 tuple

>元组，是一个**有序**且**不可变**的容器，元素可以是多种不同的数据类型
>
>1. 元组的元素个数不能修改
>
>2. 元组的元素也不能被替换成其他的值
>
>**注意：元组中的列表可以进行增删改元素的操作，即该列表自身内部的修改**

```python
定义元组：
1. item = ()
2. item = tuple()
```

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
	6. in是否包含
```

>**注意：元组中只有1个元素时，该元素后要添加 ','**

```python
v1 = (1,)    # v1是元组
v2 = (1)     # v2 = 1
```


### 字典 dict

>字典是一个**无序**、**键不重复**且**元素只能是键值对**的**可变**的容器
>
>1. **Python3.6之前，字典无序的；Python3.6之后字典有序**
>
>2. **键重复时数据会被覆盖**
>
>3. **键必须是可哈希类型**
>
>	可哈希：int、bool、str、tuple
>	
>	不可哈希：list、dict

```python
定义字典：
1. item = {}
2. item = dict()
```

```python
独有功能：
	1. 根据键获取值：get()   # 键不存在时返回 None 或传入的默认值
	2. 获取所有的键：keys()  
	 # 结果是一个高仿列表，dict_keys([...])，支持for循环，可以直接使用list()将其转为列表
	3. 获取所有的值：values()
	 # 结果是一个高仿列表，dict_values([...])，支持for循环，可以直接使用list()将其转为列表
	4. 获取所有的键值：items()
	 # 结果是一个高仿列表，dict_items([()...()])，每个元素是一个元组，元组中封装键和值
	 # 支持for循环，for循环支持双变量
```

```python
公共功能：
	1. 长度：len()
	2. 索引：dict[key]
	 # 与列表、字符串、元组不同，字典的索引是键，而不是0,1,2...
	 # 键不存在会报错
	 # 支持通过索引增删改 del
	3. for循环遍历元组中的每个元素
	4. 嵌套
	5. in是否包含 # 判断键是否在字典中
```


### 集合 set

>集合是一个**无序**、**可变**、**元素必须可哈希**且**元素不重复**的容器
>
>应用情景：不希望数据重复时使用

```python
定义集合：
1. item = set()
```

```python
独有功能：
	1. 添加元素：add()
	2. 删除元素：discard()   # 若元素不存在，不会报错
	3. 交集：
		   方法1：set1.intersection(set2)  # 生成新的集合
		   方法2：set1 & set2
	4. 并集：
		   方法1：set1.union(set2)
		   方法2：set1 | set2
	5. 差集：
		   方法1：set1.difference(set2)  # set1中有的，但set2中没有的元素
		   方法2：set1 - set2            # set1中有的，但set2中没有的元素
```

```python
公共功能：
	1. 长度：len()
	2. for循环遍历集合中的每个元素，但无序
	3. in是否包含
	  # 判断元素是否在集合中
	  # 集合中的元素可哈希，效率高，速度快
```

集合的存储：

![](assets/Python基础/file-20251203232511349.png)

**容器之间的转换**

>列表、元组、集合之间可以进行相互转换
>
>**原则：想转换成谁就用谁的英文名包裹起来：list()、tuple()、set()**
>
>**注意：列表 / 元组 => 集合，会自动去重**


### None类型

>**None 表示空值，相当于其他语言中的 null**



## 文件操作

>**操作对象：普通文本文件**
>
>**基本步骤：**
>
>	**1. 打开文件**
>	
>	**2. 操作文件**
>	
>	**3. 关闭文件**

### 写文件

#####  1. 覆盖写入

```python
# 1. 打开文件  
# hello.txt 文件路径  
# mode = 'wb' 以写模式打开文件：文件不存在，则创建文件；文件存在，则清空文件（覆盖写入）
	# w - 覆盖写入
	# b - 字节类型
file_object = open('hello.txt',mode = 'wb')  
  
# 2. 写入内容  
hello = 'hello world'  
file_object.write( hello.encode('utf-8') )  
  
# 3. 关闭文件  
file_object.close()
```

练习题：用户注册，每注册一个用户就在文件中写入一行"用户名,密码"（循环操作，Q/q终止）

```python
# 覆盖写入
file_object = open('user_info.txt',mode = 'wb')  
  
while True:  
    name = input('请输入用户名：')  
    if name.upper() == 'Q':  
        break  
    password = input('请输入密码：')  
  
    user_info = '{},{}\n'.format(name,password)  
  
    # 延时写入：暂时将数据写入计算机内存中（缓冲区），文件关闭后再一次性移动到硬盘（文件）中  
    file_object.write( user_info.encode('utf-8') )  
  
    # 实时写入：强制将内存（缓冲区）中的数据写入硬盘（文件）中  
    file_object.flush()  
  
file_object.close()
```

##### 2. 追加写入

```python
# 1. 打开文件  
# hello.txt 文件路径  
# mode = 'ab' 以写模式打开文件：文件不存在，则创建文件；文件存在，则在文件尾部追加写入
	# a - 追加写入
	# b - 字节类型
file_object = open('hello.txt',mode = 'wb')  
  
# 2. 写入内容  
hello = 'hello world'  
file_object.write( hello.encode('utf-8') )  
  
# 3. 关闭文件  
file_object.close()
```

**练习题：用户注册，每注册一个用户就在文件中写入一行"用户名,密码"（循环操作，Q/q终止）**

```python
# 追加写入
file_object = open('user_info.txt',mode = 'ab')  
  
while True:  
    name = input('请输入用户名：')  
    if name.upper() == 'Q':  
        break  
    password = input('请输入密码：')  
  
    user_info = '{},{}\n'.format(name,password)  
  
    # 暂时将数据写入计算机内存中（缓冲区），文件关闭后再一次性移动到硬盘（文件）中  
    file_object.write( user_info.encode('utf-8') )  
  
    # 实时写入：强制将内存（缓冲区）中的数据写入硬盘（文件）中  
    file_object.flush()  
  
file_object.close()
```


### 读文件

##### 1. 读取文件全部内容

```python
# 1. 打开文件  
# hello.txt 文件路径  
# mode = 'rb' 以读模式打开文件  
	# r - 读  
	# b - 字节类型  
file_object = open('hello.txt', mode = 'rb')  
  
# 2. 读取文件的所有内容  
content = file_object.read()  
print(content) # b'hello world' - 字节类型  

# 将字节类型解码为字符串类型
content_string = content.decode('utf-8')  
print(content_string) # hello world - 字符串类型  
  
# 3. 关闭文件  
file_object.close()
```

##### 2. 逐行读取

>读取大文件时，可以逐行读取

**方法1：使用 `readline()`**

```python
# 1. 打开文件  
# hello.txt 文件路径  
# mode = 'rb' 以读模式打开文件  
    # r - 读  
    # b - 字节类型  
file_object = open('hello.txt', mode = 'rb')  
  
# 2. 逐行读取文件内容  
# 读取文件第一行  
line1 = file_object.readline()  
print(line1.decode('utf-8').strip())  
# 读取文件第二行  
line2 = file_object.readline()  
print(line2.decode('utf-8').strip())  
# 读取文件第三行  
line3 = file_object.readline()  
print(line3.decode('utf-8').strip())  
# ... ...  
  
# 3. 关闭文件  
file_object.close()
```

**方法2：使用 `for` 循环**

```python
# 1. 打开文件  
# hello.txt 文件路径  
# mode = 'rb' 以读模式打开文件  
    # r - 读  
    # b - 字节类型  
file_object = open('hello.txt', mode = 'rb')  
  
# 2. 逐行读取文件内容  
for line in file_object:  
    line_string = line.decode('utf-8') # 将字节类型解码为字符串类型  
    line_string = line_string.strip()  # 去掉每行字符串前后的换行 \n    print(line_string)  
  
# 3. 关闭文件  
file_object.close()
```

**练习题1：读取文件每行中的单词**

```txt
q,quit,12,1  
w,wait,122,122  
e,equal,12,313  
  
  
s,seek,12222,2

```

```python
# 读模式打开文件
file_object = open('data.txt',mode = 'rb')  

# 逐行遍历
for line in file_object:  
    line_string = line.decode('utf-8').strip()  # 解码，并去除尾部换行符
    
    # '\n' => ''，若为空行，什么也不做；不为空行，输出本行中的单词
    if line_string:  
        word = line_string.split(',')[1]  
        print(word)  

# 关闭文件
file_object.close()
```

```txt
quit
wait
equal
seek
```

**练习题2：实现用户注册和用户登录功能**

```python
print('========== 用户注册 ==========')  
  
# 写文件  
register_file = open('user_data.txt', mode='ab')  
  
while True:  
    name = input('请输入姓名：')  
    if name.upper() == 'Q':  
        break  
    password = input('请输入密码：')  
  
    user_info = '{},{}\n'.format(name, password)  
    register_file.write(user_info.encode('utf-8'))  
  
# 关闭文件  
register_file.close()  
  
print('========== 用户登录 ==========')  
  
# 输入登录用户名和密码  
user_name = input('用户名：')  
user_password = input('密码：')  
  
output_text = '用户名或密码错误' # 输出信息  
  
# 读文件  
login_file = open('user_data.txt', mode ='rb')  
  
# 逐行遍历  
for line in login_file:  
    # line = '用户名,密码\n'  
    line_string = line.decode('utf-8').strip()  # 解码，并去除尾部换行符  
    info = line_string.split(',')  
    if info[0] == user_name and info[1] == user_password:  
        output_text = '登录成功'  
  
print(output_text)  
  
# 关闭文件  
login_file.close()
```


### with上下文管理

>**注意：打开文件后一定要记得关闭文件，否则会造成资源浪费**
>
>**with上下文管理，让你告别忘记关闭文件：**
>
>	离开 `with` 的缩进后，会自动调用 `file_object.close()` 关闭文件 

```python
print('开始')  
  
# 离开缩进后，会自动调用file_object.close()关闭文件  
with open('hello.txt', mode = 'rb') as file_object:  
    data = file_object.read().decode('utf-8')  
    print(data)  
  
print('结束')
```


### 文件打开模式

- `wb`，写

```python
file_object = open('hello.txt',mode = 'wb')  

file_object.write( '你好'.encode('utf-8') )  

file_object.close()
```

- `w`，写

>**注意：需要设置 `encoding` 参数，会实现自动 `encode` 压缩编码**

```python
file_object = open('hello.txt',mode = 'w',encoding = 'utf-8')  

file_object.write( '你好' )  

file_object.close()
```

- `ab`，追加

```python
file_object = open('hello.txt',mode = 'ab')  

file_object.write( '你好'.encode('utf-8') )  

file_object.close()
```

- `a`，追加

>**注意：需要设置 `encoding` 参数，会实现自动 `encode` 压缩编码**

```python
file_object = open('hello.txt',mode = 'a',encoding = 'utf-8')  

file_object.write( '你好' )  

file_object.close()
```

- `rb`，读

```python
file_object = open('hello.txt',mode = 'rb')  

# 读取到的是被压缩成 utf-8 编码的字节类型数据
data = file_object.read()
# utf-8编码（字节类型） -> unicode编码（字符串类型）
data_string = data.decode('utf-8')
print(data_string)

file_object.close()
```

- `r`，读

>**注意：需要设置 `encoding` 参数，会实现自动 `decode` 解码**

```python
file_object = open('hello.txt',mode = 'r',encoding = 'utf-8')  

# 直接读取到的是 unicode 编码的原始字符串，不需要再 decode 解码
data = file_object.read()
print(data_string)

file_object.close()
```
