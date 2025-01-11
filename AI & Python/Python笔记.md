> 来自《Python编程：从入门到实践》

### 注释

单行注释(‘#’)       多行注释(三个单引号 或 三个双引号)

```python
# 你好，这是一个单行注释
'''
这是一段多行注释
'''
```



## 数据类型

### 变量

使用声明变量时无需注明其类型！！！      Python中变量无类型，其存储的数据有类型。

```python
message = "hello"   #创建一个叫message的变量
age = 23
```

```python
type()   # 使用type()可查看数据类型
```



### 字符串 String

用引号括起，可以是单引号也可以是双引号。   **字符串是不可变数据**。                  

注：在用单引号括起的字符串中，如果包含撇号，就将导致错误。因为导致Python将第一个单引号和撇号之间的内容视为一个字符串。

```python
message = "hello "    # √
message = ' gg '      # √
message = " I'm your father 'dada'! "   # √    撇号可处在两个双引号里面； 或用转义字符\"  
message = '''
你好，我是
有效字符串
'''                   # √ 三引号定义法：写法与多行注释一样，用变量去接收就是字符串，否则就是注释。

print("my father is ", 85, " years old")   # print中用逗号(,)将不同内容隔开
```

方法：

```python
# 大小写转换
title()   # 以首字母大写方式显示每个单词       name.title()  : name="ada love"  → Ada Love
upper()   # 所有字母全转为大写				name.upper()  : name="hello" → HELLO
lower()   # 所有字母全转为小写

# 合并(拼接)字符串 ： '+'        message = "hello " + " " + "there"   || '+'号连接必须保证两侧均是字符串！
# 空白 [包括 空格、制表符、换行符]
制表符 \t   换行符 \n    # print("hello\n\tPython")

# 删除空白
rstrip()  # 删除字符串末尾所有空白          name.lstrip() : name="hello   " → "hello"
lstrip()  # 删除字符串开头所有空白
strip()   # 同时删除字符串两端所有空白
	#注：对字符串变量进行修改后，想要保存新值需要把处理结果再重新存回变量
```

**字符串格式化：**在 Python 中，字符串格式化使用与 C 中 sprintf 函数一样的语法。   用 `%` 来占位。

```py
print("My name is %s , I'm %d years old now!" % ("xiaoMing", 15))    # 多个变量占位，变量要用括号括起来,并按顺序填入 

%c：格式化字符及其ASCII码     %s：格式化字符串             %d：将内容转换成整数,放入占位     %u：格式化无符号整型
%o：格式化无符号八进制数       %x：格式化无符号十六进制数   %X：格式化无符号十六进制数（大写）  
%f：格式化浮点数字，可指定小数点后的精度   %e：用科学计数法格式化浮点数   %E：作用同%e，用科学计数法格式化浮点数
%g：%f和%e的简写             %G：%f 和 %E 的简写          %p：用十六进制数格式化变量的地址

# "m.n" 控制数据宽度和精度
%5d  [宽度为5]
%5.3f  [宽度为5(小数点与小数也会计入宽度)；  小数点精度为3，小数会四舍五入]

# 快速格式化：使用 f"内容{变量}"     【不关心数据类型，不关心精度】
name = "xiaoMing"
print(f"My name is {name}, I'm {15.5} years old!")
```



### 数字 Number

**数字是不可变数据。 **Python3 支持 **int、float、bool、complex（复数）**。

整数 int：     [Python3中使用除法无法整除时会得到一个浮点数]   只有一种整数类型 int，表示为长整型.

```python
# 加(+)    减(-)    乘(*)      除(/)           乘方(**)  
# 2 + 3    # 2-5   # 3 * 8   # 5/2 =2.5      # 3**3 =27
```

​	注：Python3 中，bool 是 int 的子类，True 和 False 可以和数字相加，**True == 1 , False==0** *会返回* True，但可以通过 is来判断类型。

布尔型 bool：    `True 、 False`

浮点数 float：带小数点的数字。  [小数点可出现在数字的任何位置]    很大程度上使用浮点数无需考虑其行为。

​	注：结果包含的小数位数可能不确定。如  `0.2 + 0.1 = 0.30000000000004`

复数 complex：复数由实数部分和虚数部分构成，可以用 **a + bj**，或者 **complex(a,b)** 表示， 复数的实部 **a** 和虚部 **b** 都是浮点型。

```python
int: 10	, 0x69   float: 0.0 , 70.2E-12	 complex: 3e+26J , 3.14j , 9.322e-36j
```



#### 显式类型转换

```python
str()      # 将非字符串值表示为字符串。  [用在字符串打印时]   print("hello" + str(age) + "birthday")
int(x)     # 将x转换成整数.
float()
```



### 列表 List

列表由一系列按特定顺序排列的元素组成.   用方括号（[]）来表示列表，并用逗号来分隔其中的元素。 `[‘trek’, ‘redline’]`

列表适合用于存储在程序运行期间可能变化的数据集。**列表是可以修改的**。Python将不能修改的值称为`不可变的`，而`不可变的列表`被称为`元组`。

**访问：**  **列表是有序集合**。   使用元素的位置或索引即可。       [列表中元素可以不是同种类型]

```python
bicycles = ['trek', 'redline', 'specialized']
print(bicycles[0])     # 返回 trek     || 只返回该元素
print(bicycles[-1])    # 返回 specialized
```

【索引从0开始从前往后0,1,2,...；  从后往前是 -1, -2, ...】

**修改：**指定列表名和要修改的元素的索引，再指定该元素的新值。

```python
bicycles[0] = 'honda'   # 变为 bicycles = ['honda', 'redline', 'specialized']
```

**添加:**  分为 在末尾添加 和 在中部插入

```python
append()   # 将元素添加到列表末尾.             motors = []    motors.append('yamaha')
insert()   # 在列表任意位置添加新元素.    motors = ['honda', 'yamaha']  motors.insert(0, 'ducati') ducati变为0号
```

**删除：**

```python
del      # 删除指定位置的元素.     del bicycles[0]   变为 bicycles = ['redline', 'specialized']
pop()    # 删除列表末尾的元素，并让你能够使用该元素.{类似弹出栈顶元素}   ele = motors.pop()    则ele='yamaha'
pop(5)   # 也可指定索引来删除任意位置元素，并返回该元素供使用.
remove() # 不知道从列表中删除的值所处位置,只知道要删除的元素值.   bicycles.remove('redline')    
	#注:方法remove()只删除第一个指定的值。如果要删除的值在列表中出现多次，就需要使用循环来判断是否删除了所有这样的值.
```

**排序：**  [列表中元素必须全为同一类型，才能排序]

```python
sort()  # 将列表中字符串按字母顺序或数字按从小到大顺序排序，永久性修改了列表元素的排列顺序。
	# cars=['bmw','audi','toyota']   cars.sort()   变为 cars=['audi','bmw','toyota']
sort(reverse = True) # 将列表中字符串按字母反向顺序或数字按从大到小顺序排序,永久性修改了列表元素的排列顺序。
	# nums=[12,35,20]   nums.sort(reverse=True)  变为 nums=[35,20,12]
    
sorted()   # 按特定顺序显示列表元素，同时不影响它们在列表中的原始排列顺序. 
	# cars=['bmw','audi','toyota']  print(sorted(cars)) 打印出['audi','bmw','toyota'] 但cars列表内容不变。
sorted( ,reverse = True)    
	# print(sorted(cars, reverse=True))
    
reverse()   # 反转列表元素的排列顺序. 永久性修改列表元素排列顺序。
	# reverse()不是指按与字母顺序相反的顺序排列列表元素，而只是反转列表元素的排列顺序：
    #   cars=['bmw','audi','toyota']   cars.reverse()  变为 ['toyota', 'audi', 'bmw']
```

**获取长度： **计算列表元素数时从1开始。

```py
len()   # 获取列表长度。     cars=['bmw','audi','toyota']  print(len(cars))   打印出 3  
```

#### 列表进阶-遍历&数值表

##### for循环

**遍历：**  在for循环后面，没有缩进的代码都只执行一次，而不会重复执行。   for语句末尾的冒号告诉Python，下一行是循环的第一行。

```py
magicians = ['alice','bob','chris']
for magic in magicians:   # for循环遍历, 注意末尾的冒号：
    print(magic)     #注意缩进
    print(magic.title() + "is good.")
    
print("Thank you all.")  # 不在循环里执行   
```

**注意缩进:**Python根据缩进来判断代码行与前一个代码行的关系。  对于位于for语句后面且属于循环组成部分的代码行，一定要缩进。



**创建数值列表：**使用函数range()几乎能够创建任何需要的数字集.

```py
range()  # range()让Python从你指定的第一个值开始数，并在到达你指定的第二个值后停止，因此输出不包含第二个值.
'''
	for value in range(1, 5):
		print(value)      #最终打印出 1 2 3 4 
'''
list()   # 将一堆元素转换为一个列表;   可结合range()创建数字列表
	# numbers = list(range(1, 6))       则 numbers=[1,2,3,4,5]

range(, , )    # 还可指定步长。
	# range(2,11,2) 指函数range()从2开始数，然后不断地加2，直到达到或超过终值（11）；  最终得到 2,4,6,8,10
```

```py
#  Python提供有专门用于处理数字列表的函数     #

min()   #               digits=[1,2,3,4,5,0]       min(digits)
max()	#                         max(digits)
sum()	#						sum(digits)
```

**列表解析：** 列表解析将for循环和创建新元素的代码合并成一行，并自动附加新元素。

​                  [当觉得编写三四行代码来生成列表有点繁复时，就应考虑创建列表解析了]

```python
squares = [value**2 for value in range(1,11)]   #生成一个平方数列表； [这里for循环末尾无冒号]
print(squares)
```

首先指定一个描述性的列表名；然后指定一个左方括号，并定义一个表达式，用于生成你要存储到列表中的值。接下来编写一个for循环，用于给表达式提供值，再加上右方括号。                请注意，**这里的for语句末尾没有冒号**。



**切片：**包含列表的部分元素的列表。 创建切片，可指定要使用的第一个元素和最后一个元素的索引; (最后一个元素不包含)

```python
players = ['Alan', 'Bob', 'Charles', 'David']
players[0:2]   # 创建包含第0个和第1个元素的列表； 即['Alan', 'Bob']
players[1:3]
players[:2]    # 未指定第一个索引，自动从列表开头开始.
players[1:]    # 未指定终止索引，自动终止于列表末尾.

# 负数索引返回离列表末尾相应距离的元素.
players[-3:]   # 即是倒数第3个元素一直到末尾；  即['Bob','Charles','David']

# for循环
for player in players[1:3] :
    #####
```

**复制列表：**要复制列表，可创建一个包含整个列表的切片，方法是同时省略起始索引和终止索引（[:]）。 【即使用切片来复制列表】

```python
my_foods = ['pizza','apple', 'cake']
your_foods = my_foods[:]   # 在不指定任何索引的情况下从my_foods中提取一个切片，从而创建了这个列表的副本，再将该副本存储到变 							量friend_foods中
	# 注：不要写成 your_foods = my_foods   # 这是让新变量friend_foods关联到my_foods列表，因此这两个变量都指向同一个列表。
```



### 元组 Tuple—不可变的列表

不能修改的值称为不可变的，而不可变的列表被称为元组。元组看起来犹如列表，但使用圆括号来标识。定义元组后可以使用索引来访问其元素，就像访问列表元素一样。

```python
dimensions = (200, 50)    # 创建有两个元素的元组
print(dimensions[0])      # 打印出 200 ，即第一个元素

	# 注: dimensions[0] = 250   运行时Python会报错，因为禁止修改元组元素
    
for dimension in dimensions:  #for循环    
```

虽然不能修改元组的元素，但可以给存储元组的变量赋值。

```python
dimensions = (200, 50)
dimensions = (400, 100)  # 将一个新元组存储到变量中。  给元组变量赋值是合法的。
```



### 字典 Dictionary

**字典是可变数据。**字典是一系列`键-值对`。用放在花括号`{ }`中的一系列`键-值对`表示。每个键都与一个值相关联，你可以使用键来访问与之相关联的值。与键相关联的值可以是数字、字符串、列表乃至字典。事实上，可将任何Python对象用作字典中的值。

键和值之间用冒号分隔，而键—值对之间用逗号分隔。   存储多少个键—值对都可以。 【同一个字典中存的值可以是不同类型】

```python
alien_o = {'color': 'green', 'points':5}
print(alien_o['color'])

alien_dic = {}   # 创建一个空字典

liked_lang = {          # 使用多行来定义一个字典
    'jane': 'python',
    'sarah': 'c++' ,    # 缩进量同第一个键值对
    'john': 'java' ,    # 可在最后一个键—值对后面也加上逗号，为以后在下一行添加键—值对做好准备。
	}             	    # 右花括号缩进量与字典中的键对齐
```

**添加:**要添加键-值对，可依次指定字典名、用方括号括起的键和相关联的值。

注意，键-值对的排列顺序与添加顺序不同。Python不关心键—值对的添加顺序，而只关心键和值之间的关联关系。

```python
alien_o = {'color': 'green', 'points':5}
alien_o['x_position'] = 25    # 添加
```

**修改：**要修改字典中的值，可依次指定字典名、用方括号括起的键以及与该键相关联的新值

```python
alien_o = {'color': 'green', 'points':5}
alien_o['color'] = 'yellow'  # 修改
```

**删除：**使用del语句将相应的键-值对彻底删除。使用del语句时，必须指定字典名和要删除的键。

```python
del    #
alien_o = {'color': 'green', 'points':5}
del alien_o['points']   # 删除指定键-值对
```

**遍历**

```python
user_dic = {'username': 'efer', 'first': 'enrico', 'last': 'fremi'}
# 遍历所有键值对     使用 items() ; 方法items()返回一个键-值对列表
for key, value in user_dic.items():   # 声明两个变量，用于存储键—值对中的键和值。对于这两个变量，可使用任何名称
    print("\nKey: " + key)
    print("\nValue: " + value)

    # 注：遍历字典时，键—值对的返回顺序也与存储顺序不同。   即获取字典的元素时，获取顺序是不可预测的。

# 遍历所有键         使用 keys() ; 方法keys()并非只能用于遍历；实际上，它返回一个列表，其中包含字典中的所有键。
for k in user_dic.keys():  # 注：遍历字典时，会默认遍历所有的键。所以也可省略.keys()
    print("\nKey: " + k)

# 遍历所有值         使用 values() ;  方法values()，它返回一个值列表，而不包含任何键。
for v in user_dic.values():
    print("\nValue: " + v)

# 按顺序遍历所有键: 要以特定的顺序返回元素，一种办法是对返回的键进行排序。可用函数sorted()来获得按特定顺序排列的键列表的副本
for key in sorted(user_dic.keys()):
    
```

**嵌套：**可以在列表中嵌套字典、在字典中嵌套列表甚至在字典中嵌套字典。    [列表和字典的嵌套层级不应太多。]

```python
# 在列表中嵌套字典:
alien_0 = {'color': 'green', 'points': 5}
alien_1 = {'color': 'yellow', 'points': 10}
alien_2 = {'color': 'red', 'points': 15}
aliens = [alien_0, alien_1, alien_2]
# 在字典中嵌套列表:    每当需要在字典中将一个键关联到多个值时，都可以在字典中嵌套一个列表。
pizza = {
	'crust': 'thick',
	'toppings': ['mushrooms', 'extra cheese'],
	}
# 在字典中嵌套字典:
users = {
	'aeinstein': {
		'first': 'albert',
		'last': 'einstein',
		'location': 'princeton',
		},
	'mcurie': {
		'first': 'marie',
		'last': 'curie',
		'location': 'paris',
		},
	}
```



### 集合 Set—无重复元素的列表



## 用户输入 input()

函数`input()`让程序暂停运行，等待用户输入一些文本。 [ 使用函数input()时，Python将用户输入解读为字符串。]

```python
my_message = input()    # 输入被解读为字符串。 若想将输入作为数字使用，要用int()或其他函数将输入转换成对应类型。
	# 注：将数值输入用于计算和比较前，务必将其转换为数值表示。
    
# 函数input()接受一个参数(也可以是变量)：即要向用户显示的提示或说明，让用户知道该如何做.
message = input("Tell me something, and I will repeat it back to you: ") #会把参数内容显现出来，再等待输入
print(message)
```



## if-elif-else & for & while

### if语句

每条if语句的核心都是一个值为True或False的表达式，这种表达式被称为`条件测试`。如果条件测试的值为True，Python就执行紧跟在if语句后面的代码；如果为False，Python就忽略这些代码。

【在if语句中将列表名用在条件表达式中时，Python将在列表至少包含一个元素时返回True，并在列表为空时返回False】

```python
for car in cars:
    if car == 'bmw':       # 注意末尾冒号      【检查相等时会考虑大小写】
		print(car.upper())
    else:                  # 注意末尾冒号
    	print(car.title())
```

#### 运算符

```python
#  【 Python 中是没有 ++ 和 -- 的】
        
#    等于：==     不等于：!=        
# 数学比较： < , > , <=  ,  >= 
# 算术运算符： + , - , * , / , % (取模) , ** , // ('取整除':往小的方向取整数)
# 赋值运算符： = , +=, -=, *=, /=, %=, **=, := (海象[walrus]运算符:在表达式内部为变量赋值。Python3.8新增) 
# 逻辑运算符：   (与): and    (或): or    (非): not 
# 位运算符： & (按位与) , | (按位或) , ^ (按位异或) , ~ (按位取反) , << (左移) , >> (右移)
# 成员运算符：  in (如果在指定的序列中找到值返回True,否则返回False) ,  not in ()
# 身份运算符：  is (判断两个变量引用对象是否为同一个) ,  is not ()

if (n := len(a)) > 10:   # walrus运算符 [先赋值再判断]  即先让n=len(a), 再判断 if(n > 10):
    #...
if not( a and b ):
    #...
if ( a or b ):
    #...
    
a = 10
list=[10,20,30]
if ( a in list ):
    #...

a = 20
b = 20 
if ( a is b ):    # 这里a和b是引用同一个对象    【 id()函数用于获取对象内存地址;  如 if(id(a) == id(b)):】
    #...
```

**简单if语句**

```python
if conditional_test:
	do_something
```

**if-else / if-elif-else语句**

```python
if conditional_test:
    do_sth
else:
    do_another_sth
    
# if-elif-else   [if-elif-else仅适合用于只有一个条件满足的情况：遇到通过了的测试后，Python就跳过余下的测试]
if age < 4:
    #...
elif age < 8:
    #...
elif age < 15:
    #...
else:            # else语句可省略
    #...
```

### for循环

在for循环后面，没有缩进的代码都只执行一次，而不会重复执行。   for语句末尾的冒号告诉Python，下一行是循环的第一行。

```py
magicians = ['alice','bob','chris']
for magic in magicians:   # for循环遍历, 注意末尾的冒号：
    print(magic)     #注意缩进
    print(magic.title() + "is good.")
    
print("Thank you all.")  # 不在循环里执行   
```

**注意缩进:**Python根据缩进来判断代码行与前一个代码行的关系。  对于位于for语句后面且属于循环组成部分的代码行，一定要缩进。

### while语句

```python
cur_num = 1
while cur_num <= 5:    # 注意末尾冒号
    print(cur_num)
    cur_num += 1
```

使用 `break` 可跳出循环。    在任何Python循环中都可使用break语句。

```python
while True:
	city = input(prompt)
	if city == 'quit':
		break
```

使用 `continue` 直接返回到循环开头。

```python
while current_number < 10:
	current_number += 1
	if current_number % 2 == 0:
		continue
```



## 函数

函数是带名字的代码块，用于完成具体的工作。 要执行函数定义的特定任务，可调用该函数。

```python
def greet_user():      # 用 def 来进行函数定义【定义以冒号结尾】;  def 函数名(函数参数):   
    '''文档字符串-注释：描述函数的作用'''
	print("Hello!")
	# 注： 紧跟在def greet_user():后面的所有缩进行构成了函数体。  

greet_user()    # 调用函数

def des_pet(pet_name):    # 带参函数
    print(pet_name)

des_pet('doggy')    
```

**实参、形参：** 形参—函数完成其工作所需的一项信息。 实参—调用函数时传递给函数的信息。

### 参数传递

**1.位置实参：**将函数调用中的每个实参都关联到函数定义中的一个形参。基于实参的顺序进行关联的方式被称为位置实参。

```python
def describe_pet(animal_type, pet_name):
	"""显示宠物的信息"""
	print("\nI have a " + animal_type + ".")
	print("My " + animal_type + "'s name is " + pet_name.title() + ".")

describe_pet('hamster', 'harry')    # 按照顺序把'hamster'对应到animal_type; 'harry'对应到pet_name
```

**2.关键字实参：**关键字实参是传递给函数的`名称-值对`。直接在实参中将名称和值关联起来，因此向函数传递实参时不会混淆。关键字实参无需考虑函数调用中的实参顺序，还清楚地指出了函数调用中各个值的用途。

```python
def describe_pet(animal_type, pet_name):
	"""显示宠物的信息"""
	print("\nI have a " + animal_type + ".")
	print("My " + animal_type + "'s name is " + pet_name.title() + ".")

describe_pet(animal_type='hamster', pet_name='harry')   # 关键字实参传递  【关键字实参的顺序无关紧要】
```

**3.默认值：**编写函数时，可给每个形参指定默认值。在调用函数中给形参提供实参时，Python将使用指定实参值；否则将使用形参默认值.    

【使用默认值时，在形参列表中必须先列出没有默认值的形参，再列出有默认值的实参。让Python依然能正确解读位置实参。】

```python
def describe_pet(pet_name, animal_type='dog'):
	"""显示宠物的信息"""
	print("\nI have a " + pet_name ":"+ animal_type + ".")

describe_pet(pet_name='willie')    # 省略默认值，则animal_type为默认值
describe_pet('willie')             # 位置实参情况下省略默认值，Python仍按位置顺序传入参数与使用默认值
describe_pet(pet_name='harry', animal_type='hamster')   # 提供完整参数，就不考虑默认值
```

**可使用默认值来让实参变成可选的：**可给实参变量指定一个默认值——空字符串，并在用户没有提供参数时不使用这个实参。

```python
def get_formatted_name(first_name, last_name, middle_name=''):    # 使用默认值，且默认值为空
	"""返回整洁的姓名"""
	if middle_name:
		full_name = first_name + ' ' + middle_name + ' ' + last_name
	else:
		full_name = first_name + ' ' + last_name
	return full_name.title()

musician = get_formatted_name('jimi', 'hendrix')
```

### 返回值

函数返回的值被称为`返回值`。在函数中，可使用return语句将值返回到调用函数的代码行。 函数可返回任何类型的值，包括列表和字典等较复杂的数据结构。  

```python
def get_formatted_name(first_name, last_name):
	"""返回整洁的姓名"""
	full_name = first_name + ' ' + last_name
	return full_name.title()      # 有返回值

musician = get_formatted_name('jimi', 'hendrix')  # 调用返回值的函数时，需要提供一个变量，用于存储返回的值。
```

### 传递列表

将列表传递给函数后，函数就能直接访问其内容。    在**函数中对这个列表**所做的**任何修改**都是**永久性**的。

```python
def print_models(unprinted_designs, completed_models):
	while unprinted_designs:             # 非空就是True
		current_design = unprinted_designs.pop()
		# 模拟根据设计制作3D打印模型的过程
		print("Printing model: " + current_design)
		completed_models.append(current_design)    # 对completed_models的修改是永久性的

def show_completed_models(completed_models):
	print("\nThe following models have been printed:")
	for completed_model in completed_models:
		print(completed_model)
        
unprinted_designs = ['iphone case', 'robot pendant', 'dodecahedron']
completed_models = []
print_models(unprinted_designs, completed_models)   # completed_models会被修改
show_completed_models(completed_models)        
```

**禁止函数修改列表：**可向函数**传递列表的副本**而不是原件；这样函数所做的任何修改都只影响副本，而丝毫不影响原件。

切片表示法 [:] 创建列表的副本。

```python
print_models(unprinted_designs[:], completed_models)    # 这样unprinted_designs列表本身就不会被修改，而是副本被修改。
	# 注：是否传递副本要结合具体情况考虑。
```

### 传递任意数量实参 [*&**]

Python允许函数从调用语句中收集任意数量的实参。在形参名前**使用星号`(*)` 来创建对应一个空元组**,并把传入的任意数量实参都封装其中

```python
def make_pizza(*toppings):   # 形参名*toppings中的 * 创建一个名为toppings的空元组,将收到的所有值都封装到该元组中
	print(toppings)
    
make_pizza('pepperoni')    # Python将实参封装到一个元组中，即便函数只收到一个值也如此。
make_pizza('mushrooms', 'green peppers', 'extra cheese')
```

**Python先匹配位置实参和关键字实参，再将余下的实参都收集到最后一个形参中。** 

要让函数接受不同类型的实参，必须在函数定义中将接纳任意数量实参的形参放在最后。

```python
def make_pizza(size, *toppings):     # 
	print("\nMaking a " + str(size) +"-inch pizza with the following toppings:")

make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')    #
```

可将函数编写成能够接受任意数量的键—值对——调用语句提供了多少就接受多少。 在形参名前**使用两个星号`(**)`来创建一个空字典**，并将收到的所有`名称-值`对都封装到这个字典中。

```python
def build_profile(first, last, **user_info):   # 形参**user_info中的 ** 创建一个名为user_info的空字典，并将收到的所有
    									    # 键—值对都封装其中。
	profile = {}   # 创建一个空字典
	profile['first_name'] = first
	profile['last_name'] = last
	for key, value in user_info.items():
    	profile[key] = value
	return profile

user_profile = build_profile('albert', 'einstein', location='princeton', field='physics')  # 后两个参数是传入的键值对
```

### 格式要求

- 应给函数指定描述性名称，且只在其中使用小写字母和下划线。    多个函数上下之间用两个空行将相邻函数分开。
- 每个函数都应包含简要地阐述其功能的注释，该注释应紧跟在函数定义后面，并采用文档字符串格式.
- 给形参指定默认值时，等号两边不要有空格;    对于函数调用中的关键字实参，也应遵循这种约定.
- 所有的import语句都应放在文件开头。除非开头是注释。

```python
def function_name(
		parameter_0, parameter_1, parameter_2,   # 形参太多超行时，缩进要将形参列表和只缩进一层的函数体区分开来。
		parameter_3, parameter_4, parameter_5):
	function body...    # 函数体只缩进一层，形参每行缩进两层，要区分开。
```



## 函数模块:导入(import)

将函数存储在被称为`模块`的独立文件中，再将模块导入到主程序中。`import语句`允许在当前运行的程序文件中使用模块中的代码。通过将函数存储在独立的文件中，可隐藏程序代码的细节，将重点放在程序的高层逻辑上。还能让你在众多不同的程序中重用函数。将函数存储在独立文件中后，可与其他程序员共享这些文件而不是整个程序。    导入函数还能让你使用其他程序员编写的函数库。

**模块是扩展名为.py的文件**，包含要导入到程序中的代码。

```python
#   pizza.py 文件 —— 创建模块  #
def make_pizza(size, *toppings):
	print("\nMaking a " + str(size) + "-inch pizza with the following toppings:")
	for topping in toppings:
		print("- " + topping)

#   make.py 文件 —— 导入模块   #
import pizza           # 让Python打开文件pizza.py，并将其中的所有函数都复制到这个程序中.
pizza.make_pizza(16, 'pepperoni')    # 调用被导入的模块中的函数，要指定导入的模块的名称pizza和函数名,并用句点(.)分隔
pizza.make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')

# 1.导入整个模块： 只需编写一条import语句并在其中指定模块名，就可在程序中使用该模块中的所有函数。
import module_name
module_name.function_name()    # 使用被导入模块的函数

# 2.导入特定函数:  通过用逗号分隔函数名，可根据需要从模块中导入任意数量的函数。
from module_name import function_0, function_1, function_2
function_0()      # 使用导入的函数

from pizza import make_pizza
make_pizza(16, 'pepperoni')    # 使用这种语法,调用函数时无需使用句点.因为显式导入了函数make_pizza(),调用时只需指定其名称
make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')

# 3.使用as给函数指定别名：若要导入的函数名称可能与程序中现有名称冲突,或函数名太长;可指定简短而独特的别名(函数的另一个名称)
	# [关键字as将函数重命名为你提供的别名：]
from module_name import function_name as fn    
    
from pizza import make_pizza as mp    # 将函数make_pizza()重命名为mp()；
mp(16, 'pepperoni')                   # 在这个程序中，每当需要调用make_pizza()时，都可简写成mp()。
mp(12, 'mushrooms', 'green peppers', 'extra cheese')

# 4.使用as给模块指定别名：通过给模块指定简短的别名，能够更轻松调用模块中的函数。
import module_name as mn

import pizza as p                 # 给模块pizza指定了别名p，但该模块中所有函数的名称都没变。
p.make_pizza(16, 'pepperoni')     # 调用函数make_pizza()时，可编写代码p.make_pizza()而不是pizza.make_pizza()，
p.make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')

# 5.使用 * 导入模块中所有函数： 使用星号（*）运算符可让Python导入模块中的所有函数。
from module_name import *
  # 注：[使用并非自己编写的大型模块时，最好不要采用这种导入方法！因为可能遇到模块与项目中有名称相同的函数或变量,进而覆盖函数]
  #     [最佳的做法是，要么只导入你需要使用的函数，要么导入整个模块并使用句点表示法。 星号*导入最好不用！]
from pizza import *
make_pizza(16, 'pepperoni')  # 由于导入了每个函数，故可通过名称来调用每个函数，而无需使用句点表示法
make_pizza(12, 'mushrooms', 'green peppers', 'extra cheese')
```



## 文件

### 读取文件

要使用文本文件中的信息，首先需要将信息读取到内存中。可以一次性读取文件的全部内容，也可以以每次一行的方式逐步读取。

**注：**读取文本文件时，Python**将其中的所有文本都解读为字符串**。若读取值要将其作为数值使用，必须使用函数`int()`或`float()`转换。

```python
''' test.txt '''
3.1415926535
  8979323846
''' file.py'''						# open('pi_digits.txt')返回一个表示文件pi_digits.txt的对象并存在file_object变量中
with open('pi_digits.txt') as file_object:   # 关键字with在不再需要访问文件后将其关闭。 [with让Python自己判断何时关闭]
	contents = file_object.read()   # 方法read()读取这个文件的全部内容，并将其作为一个长字符串存储在变量contents中。
	print(contents)   # 打印末尾会多一个空行,因为read()到达文件末尾时返回一个空字符串,而将这个空字符串显示出来时就是一个空行
```

以任何方式使用文件，都得先打开文件，才能访问它。函数`open()`接受一个参数：要打开的文件的名称。Python在当前执行的文件所在的目录中查找指定的文件。函数`open()`返回一个表示文件的对象.

也可以调用open()和close()来打开和关闭文件，但这样做时，如果程序存在bug导致close()语句未执行，文件将不会关闭。

Python打开不与程序文件位于同一个目录中的文件，需要提供文件路径；分为**绝对路径**、**相对路径**。

> **相对路径**：该位置是**相对于当前运行**的程序**所在目录**的。
>
> ```python
> with open('text_files/filename.txt') as file_object:  # Linux 或 OS X
> with open('text_files\filename.txt') as file_object:  # Windows [使用反斜杠(\)]    
> ```
>
> 绝对路径：为明确地指出你希望Python到哪里去查找，你需要提供完整的路径。可将其存储在一个变量中，再传递给open()。
>
> ```python
> file_path = 'C:\Users\ehmatthes\other_files\text_files\filename.txt'  # Windows
> with open(file_path) as file_object:  
> ```

**一次性全读取：**`read()`

**逐行读取：**可使用for循环：  方法`readlines()`从文件中读取每一行，并将其存储在一个列表中；

```python
filename = 'pi_digits.txt'
with open(filename) as file_object:
	for line in file_object:
		print(line)  # 打印每一行时，都会出现空白行，因为该文件每行末尾都有一个看不见的换行符，而print语句也会加上一个换行符，因此每行末尾都有两个换行符：一个来自文件，另一个来自print语句。要消除这些多余的空白行，可在print语句中使用rstrip()。
```

使用关键字`with`时，`open()`返回的文件对象只在with代码块内可用。如果要在with代码块外访问文件的内容，可在with代码块内将文件的各行存储在一个列表中，并在with代码块外使用该列表。

```python
filename = 'pi_digits.txt'
with open(filename) as file_object:
	lines = file_object.readlines()  # 方法readlines()从文件中读取每一行，并将其存储在一个列表lines中；
for line in lines:            # 在with代码块外，依然可以使用这个变量lines。
	print(line.rstrip())
```

### 写入文件

保存数据的最简单的方式之一是将其写入到文件中。即便关闭包含程序输出的终端窗口，这些输出也依然存在。

```python
filename = 'programming.txt'
with open(filename, 'w') as file_object:   # open()提供第二个实参'w'：以写入模式打开这个文件。
	file_object.write("I love programming.")  # 使用文件对象的方法write()将一个字符串写入文件. 
                                              #  [write()不会在你写入的文本末尾添加换行符]
''''''      # 还可以使用空格、制表符和空行来设置这些输出的格式.  
with open(filename, 'w') as file_object:
	file_object.write("I love programming.\n")      # 要让每个字符串都单独占一行，需要在write()语句中包含换行符：
	file_object.write("I love creating new games.\n")

# Python只能将字符串写入文本文件。要将数值数据存储到文本文件中，必须先使用函数str()将其转换为字符串格式。    
```

**文件打开模式：**打开文件时，可指定**读取模式（'r'）**、**写入模式（'w'）**、**附加模式（'a'）**或 **能够读取和写入文件的模式（'r+'）**。           如果省略了模式实参，Python将以**默认的只读模式**打开文件。

要**写入的文件不存在，函数open()将自动创建**它。

注：以写入（'w'）模式打开文件时如果指定的文件已经存在，Python将在返回文件对象前清空该文件。                                                                    以附加（'a'）模式打开文件时，Python不会在返回文件对象前清空文件，而写入到文件的行都将添加到文件末尾。

```python
filename = 'programming.txt'
with open(filename, 'a') as file_object:       # 指定了实参'a'，以便将内容附加到文件末尾，而不是覆盖文件原来的内容.
	file_object.write("I also love finding meaning in large datasets.\n")
	file_object.write("I love creating apps that can run in a browser.\n")
```

## 异常

Python使用被称为异常的特殊对象来管理程序执行期间发生的错误。每当Python发生错误时，它都会创建一个异常对象。如果你编写了处理该异常的代码，程序将继续运行；如果你未对异常进行处理，程序将停止，并显示一个traceback，其中包含有关异常的报告。异常是使用`try-except`代码块处理的。try-except代码块让Python执行指定的操作，同时告诉Python发生异常时怎么办。使用了try-except代码块时，即便出现异常，程序也将继续运行：显示你编写的友好的错误消息。

当你认为可能发生了错误时，可编写一个try-except代码块来处理可能引发的异常.

**try-except-else代码块：**

****

```python
try:
	print(5/0)
except ZeroDivisionError:
	print("You can't divide by zero!")
   
''' 如果try代码块中的代码运行起来没有问题，Python将跳过except代码块；如果try代码块中的代码导致了错误，Python将查找这样的except代码块，并运行其中的代码.如果try-except代码块后面还有其他代码，程序将接着运行。'''

while True:
	first_number = input("\nFirst number: ")
	if first_number == 'q':
		break
	second_number = input("Second number: ")
	
    try:					  # 只有可能引发异常的代码才需要放在try语句中。
		answer = int(first_number) / int(second_number)
	except ZeroDivisionError:   # except代码块告诉Python，出现对应异常时该怎么办。
		print("You can't divide by 0!")
	else:                       # 依赖于try代码块成功执行的代码都放在else代码块中。
		print(answer)
```

Python有一个pass语句，可在代码块中使用它来让Python什么都不要做：

```python
 try:		
	answer = int(first_number) / int(second_number)
except ZeroDivisionError:   
	pass        # pass语句还充当了占位符，它提醒你在程序的某个地方什么都没有做，并且以后也许要在这里做些什么。
```

# JSON格式存数据

> JSON（JavaScript Object Notation）格式最初是为JavaScript开发的，但随后成了一种常见格式，被包括Python在内的众多语言采用。 模块json让你能够将简单的Python数据结构转储到文件中，并在程序再次运行时加载该文件中的数据。你还可以使用json在Python程序之间分享数据。更重要的是，JSON数据格式并非Python专用的，以JSON格式存储的数据能与使用其他编程语言的人分享。这是一种轻便格式。

函数json.dump()接受两个实参：要存储的数据以及可用于存储数据的文件对象。

```python
import json                          # 先导入模块

numbers = [2, 3, 5, 7, 11, 13]
filename = 'numbers.json'             # 使用文件扩展名.json来指出文件存储的数据为JSON格式。
with open(filename, 'w') as f_obj:    # 以写入模式打开这个文件，让json能够将数据写入其中。
	json.dump(numbers, f_obj)         # 函数json.dump()将数字列表存储到文件numbers.json中。
```

函数json.load()接收一个实参：用于加载数据的文件对象。

```python
import json

filename = 'numbers.json'
with open(filename) as f_obj:          # 以读取方式打开这个文件。
	numbers = json.load(f_obj)         # 函数json.load()加载存储在numbers.json中的信息，并将其存储到变量numbers中。
print(numbers)
```



# 面向对象

## 类 class

面向对象编程是最有效的软件编写方法之一。在面向对象编程中，你编写表示现实世界中的事物和情景的类，并基于这些类来创建对象。编写类时，你定义一大类对象都有的通用行为。基于类创建对象时，每个对象都自动具备这种通用行为，然后可根据需要赋予每个对象独
特的个性。根据类来创建对象被称为实例化，这让你能够使用类的实例。

### 类的创建与使用

在Python中，首字母大写的名称指的是类。类中的函数称为方法。

```python
class Dog():
	def __init__(self, name, age):   # __init__()是一个特殊的方法，每当根据Dog类创建新实例时，Python都会自动运行它。
		self.name = name             # 开头和末尾各有两个下划线，这是一种约定，避免Python默认方法与普通方法发生名称冲突。
		self.age = age      # 以self为前缀的变量都可供类中的所有方法使用。可以通过类的任何实例来访问这些变量，像这样可通过 实例访问的变量称为属性。
        
	def sit(self):
		print(self.name.title() + " is now sitting.")
        
	def roll_over(self):
		print(self.name.title() + " rolled over!")
```

在这里，方法`__init__()`定义包含了三个形参：self、name和age。其中，形参self必不可少，还必须位于其他形参的前面。  因为Python调用这个`__init__()`方法来创建Dog实例时，将自动传入实参self。每个与类相关联的方法调用都自动传递实参self，它是一个指向实例本身的引用。  self会自动传递，因此我们不需要传递它。每当我们根据Dog类创建实例时，都只需给最后两个形参（name和age）提供值。

**类中的每个属性都必须有初始值，哪怕这个值是0或空字符串**。在有些情况下，如设置默认值时，在方法`__init__()`内指定这种初始值是可行的；如果你对某个属性这样做了，就无需包含为它提供初始值的形参。

```python
class Dog():
	def __init__(self, name, age):   
		self.name = name            
		self.age = age
         self.health = 0  # 自己添加一个属性，并设置默认值
            
# 1.使用句点表示法直接修改属性值
my_dog = Dog('willie', 6)
my_dog.age = 12  #
# 2.使用类中方法修改属性值
```

### 实例的创建与使用

```python
class Dog():
	# .......
my_dog = Dog('willie', 6)  # 创建一个实例
print(my_dog.name) # 访问实例的属性，可使用句点表示法
my_dog.sit()       # 句点表示法来调用方法
```

## 继承

一个类继承另一个类时，它将自动获得另一个类的所有属性和方法；原有的类称为父类，而新类称为子类。子类继承了其父类的所有属性和方法，同时还可以定义自己的属性和方法。

```python
class Car():
	def __init__(self, make, model, year):
		self.make = make
		self.model = model
		self.year = year
		self.odometer_reading = 0
	def get_descriptive_name(self):
		long_name = str(self.year) + ' ' + self.make + ' ' + self.model
		return long_name.title()
	def read_odometer(self):
		print("This car has " + str(self.odometer_reading) + " miles on it.")
						# 创建子类时，父类必须包含在当前文件中，且位于子类前面。
class ElectricCar(Car):   # 定义子类时，必须在括号内指定父类的名称。
	def __init__(self, make, model, year):
		"""初始化父类的属性"""
		super().__init__(make, model, year) # super()特殊函数让Python调用父类的方法__init__()，
         self.battery_size = 70     # 给子类添加新属性
        
    def describe_battery(self):     # 给子类添加新方法
		"""打印一条描述电瓶容量的消息"""
		print("This car has a " + str(self.battery_size) + "-kWh battery.")  
    def read_odometer(self):       # 重写父类方法：在子类中与要重写的父类方法同名;将忽略该父类方法只关注子类中定义的方法。
		print("This car has infinite miles on it.")    
        
my_tesla = ElectricCar('tesla', 'model s', 2016)
print(my_tesla.get_descriptive_name())
```

可以将一个类的实例放在另一个类里充当属性。

## 导入类

Python允许你将类存储在模块中，然后在主程序中导入所需的模块。

**1.从一个模块中存储/导入 若干类**

```python
''' car.py '''
class Car():
    # ...
class ElectricCar(Car):
    # ...
    
''' my_car.py ''' 
from car import Car, ElectricCar     # 从一个模块中导入多个类时，用逗号分隔了各个类.

my_beetle = Car('volkswagen', 'beetle', 2016)
my_tesla = ElectricCar('tesla', 'roadster', 2016)
```

**2.导入整个模块**

```python
''' my_car.py '''
import car         # 导入整个模块，再使用句点表示法访问需要的类

my_beetle = car.Car('volkswagen', 'beetle', 2016)   # 使用语法module_name.class_name访问需要的类.
my_tesla = car.ElectricCar('tesla', 'roadster', 2016)
```

要导入模块中的每个类，可使用：`from module_name import *`；但不推荐。

**3.在一个模块中导入另一模块**

```python
''' car.py'''
class Car():
	# ...
''' electric_car.py '''
from car import Car   # 直接将Car类导入该模块中

class Battery():
    # ...
class ElectricCar(Car):
    # ...
''' my_cars.py'''
from car import Car             # 分别从每个模块中导入类
from electric_car import ElectricCar

my_beetle = Car('volkswagen', 'beetle', 2016)
my_tesla = ElectricCar('tesla', 'roadster', 2016)
```

### 导入Python标准库

Python标准库是一组模块，安装的Python都包含它。可使用标准库中的任何函数和类，为此只需在程序开头包含一条简单的import语句。

如：要创建字典并记录其中的键—值对的添加顺序，可使用模块collections中的OrderedDict类； `from collections import OrderedDict`



# 代码测试

Python标准库中的模块unittest提供了代码测试工具。单元测试用于核实函数的某个方面没有问题；测试用例是一组单元测试，这些单元测试一起核实函数在各种情形下的行为都符合要求。良好的测试用例考虑到了函数可能收到的各种输入，包含针对所有这些情形的测试。全覆盖式测试用例包含一整套单元测试，涵盖了各种可能的函数使用方式。对于大型项目，要实现全覆盖可能很难。通常，最初只要针对代码的重要行为编写测试即可，等项目被广泛使用时再考虑全覆盖。

## 函数的测试

```python
import unittest                                # 先导入模块unittest和要测试的函数get_formatted_ name()
from name_function import get_formatted_name

class NamesTestCase(unittest.TestCase):  # 自创测试类必须继承unittest.TestCase类,这样Python才知道如何运行你编写的测试。
	"""测试name_function.py"""
	def test_first_last_name(self):     # 所有以test_打头的方法都将自动运行 [由Python自动调用，不用编写调用它们的代码。]
	"""能够正确地处理像Janis Joplin这样的姓名吗？"""
		formatted_name = get_formatted_name('janis', 'joplin')
		self.assertEqual(formatted_name, 'Janis Joplin')   # 断言方法：用来核实得到的结果是否与期望的结果一致。
        
unittest.main()     # unittest.main()让Python运行这个文件中的测试。
```

## 类的测试

unittest Module中的断言方法：[ 只能在继承unittest.TestCase的类中使用这些方法 ]

|          方法           |         用途          |
| :---------------------: | :-------------------: |
|    assertEqual(a, b)    |      核实 a == b      |
|  assertNotEqual(a, b)   |      核实 a != b      |
|      assertTrue(x)      |    核实 x 为 True     |
|     assertFalse(x)      |    核实 x 为 False    |
|  assertIn(item, list)   | 核实 item 在 list 中  |
| assertNotIn(item, list) | 核实 item 不在 list中 |

```python
''' survey.py '''
class AnonymousSurvey():
	"""收集匿名调查问卷的答案"""
	def __init__(self, question):
	"""存储一个问题，并为存储答案做准备"""
		self.question = question
		self.responses = []
	def store_response(self, new_response):
	"""存储单份调查答卷"""
		self.responses.append(new_response)
            
''' test_case.py'''
import unittest
from survey import AnonymousSurvey

class TestAnonymousSurvey(unittest.TestCase):
"""针对AnonymousSurvey类的测试"""
	def setUp(self):  # unittest.TestCase类包含方法setUp()，让我们只需创建这些对象一次，并在每个测试方法中使用它们。如果在TestCase类中包含了方法setUp()，Python将先运行它，再运行各个以test_打头的方法.
	"""创建一个调查对象和一组答案，供使用的测试方法使用"""
		question = "What language did you first learn to speak?"
		self.my_survey = AnonymousSurvey(question)    # 要测试类的行为，需要创建其实例。
		self.responses = ['English', 'Spanish', 'Mandarin']
     def test_store_single_response(self):
	"""测试单个答案会被妥善地存储"""
		self.my_survey.store_response(self.responses[0])
		self.assertIn(self.responses[0], self.my_survey.responses)  # 检查English是否包含在列表中，核实是否被妥善存储
	def test_store_three_responses(self):
	"""测试三个答案会被妥善地存储"""
		for response in self.responses:
			self.my_survey.store_response(response)
		for response in self.responses:
			self.assertIn(response, self.my_survey.responses)           
unittest.main()
```

方法`setUp()`做了两件事情：创建一个调查对象；创建一个答案列表。存储这两样东西的变量名包含前缀self（即存储在属性中），因此可在这个类的任何地方使用。     可在`setUp()`方法中创建一系列实例并设置它们的属性，再在测试方法中直接使用这些实例。

> **注：**运行测试用例时，每完成一个单元测试，Python都打印一个字符：测试通过时打印一个句点；测试引发错误时打印一个E；测试导致断言失败时打印一个F.
