---
title: Pandas使用基础
categories: 
  - Python
tags:
  - Pandas
about: 无
date: 2021-02-25 16:18:58
---

# Pandas使用基础

<!--more-->

## 数据分析

### 数据结构

#### Series

##### 属性和方法

Series类似一种一维数组。

```python
se2 = pd.Series(data=[4, 7, -2, 8], index=['a', 'b', 'c', 'd'])
print(se2.values)    #通过属性values获取内容
print(se2.index)     #通过index获取索引
print(list(se2.iteritems()))    #具有字典特性
#检测缺失数据
pd.isnull(se2)  
pd.notnull(se2)
```

##### Series对象存取

Series对象下标运算可以同时支持位置和标签两种方式，同时支持位置切片和标签切片功能。

##### Series运算

支持Numpy数组运算

```python
print(se2[se2>0])		#布尔数组过滤
print(se2*2)			#标量乘法					
print(np.exp(se2))		#数学运算
print(se2+se3)			#操作符运算，前提相同标签元素才能运算，否则值为NaN
```

#### DataFrame

DataFrame是一个表格型的数据结构，既有行索引（保存在index），又有列索引（保存在columns）。

##### 常见属性方法

调用DataFrame()可以将多种格式的数据转换为DataFrame对象，它的的三个参数data、index和columns分别为数据、行索引和列索引。data可以是，二维数组、字典、结构数组。

```python
#直接传入一个等长列表或Numpy组成的字典。
dict1={"Province":["Guangdong","Beijing","Qinghai","Fujiang"],
      "year":[2018]*4,
      "pop":[1.3,2.5,1.1,0.7]}
df1=pd.DataFrame(dict1)
print(df1)
#创建时指定序列
df2=pd.DataFrame(dict1,columns=['year','Province','pop','debt'],index=['one','two','three','four'])
print(df2)
print(df2.shape)    #通过shape属性获取DataFrame的行数和列数
print(df2.values)	#values属性通过ndarray形式返回DataFrame的数据
```

##### DataFrame转换为其他格式的数据

to_dict()转换为字典。

to_csv()转换为csv格式。

to_records()转换为记录格式。

##### 常见存取、赋值、删除

**DataFrame_object[ ]** 能通过**列索引**来存取，当只有一个标签则返回Series，多于一个则返回DataFrame：

**DataFrame_object.loc[ ]** 能通过**行索引**来获取指定行

##### Index对象

Index对象保存着索引标签数据，它可以快速找到标签对应的整数下标，其功能与Python的字典类似。

```python
col_index=df1.columns		#返回DataFrame对象所有的列索引
col_index.values
ind_index=df1.index			#返回DataFrame对象所有的列标签
ind_index.values
```

Index对象调用Index()来创建，可传递给DataFrame对象的参数index和columns。因为Index是不可变的，因此多个DataFrame对象的索引可以是同个Index对象。

Index对象可当做一维数组，适合Numpy数组的下标运算，但Index对象只是可读，创建后不可修改。

index对象具有字典的映射功能，**.get_loc(value)**获得单值得下标，**.get_indexer(values)**获得一组值得下标，当值不存在则返回-1：

##### MultiIndex对象

MultiIndex表示多级索引，从Index继承过来的，其中多级标签用元组对象来表示。

```python
#元祖列表创建
m_index1=pd.Index([("A","x1"),("A","x2"),("B","y1"),("B","y2"),("B","y3")],name=["class1","class2"])
print(m_index1)
df1=pd.DataFrame(np.random.randint(1,10,(5,4)),index=m_index1)
print(df1)
#特定结构创建
class1=["A","A","B","B"]
class2=["x1","x2","y1","y2"]
m_index2=pd.MultiIndex.from_arrays([class1,class2],names=["class1","class2"])
df2=DataFrame(np.random.randint(1,10,(4,3)),index=m_index2)
#笛卡尔积创建
m_index3=pd.MultiIndex.from_product([["A","B"],['x1','y1']],names=["class1","class2"])
df3=DataFrame(np.random.randint(1,10,(2,4)),columns=m_index3)
```

MultiIndex对象属性，可通过get_loc()和get_indexer()获取标签的下标。

## 文件处理

### CSV文件处理

#### 读取

```python
pd.read_csv(filepath_or_buffer, sep=’,’, delimiter=None, header=’infer’, names=None, index_col=None, usecols=None, squeeze=False, converters=None, true_values=None, false_values=None, skiprows=None, nrows=None, na_values=None)
'''
filepath_or_buffer：文件名、文件具体或相对路径、文件对象
usecols：保留指定列
sep、delimiter：俩者均为文件分割符号，或为正则表达式
header：当文件中无列名需将其设为None
names：结合header=None，读取时传入列名
skiprows：忽略特定的行数
nrows：读取一定行数
na_values：一组将其值转换为NaN的特定值
sueeze：返回Series对象
'''

pd.read_csv('test.csv',usecols=[0,2])		#保留指定列
#读取无列名文件，names为自行输入列名
pd.read_csv('test2.csv',header=None,names=['k1','k2','value1','value2'])
#读入时，指定列为索引
pd.read_csv('test.csv',index_col=['k1','k2'])
#从特定行读取，若第一行为列名，会被忽略
pd.read_csv('test2.csv',header=None,names=['k1','k2','value1','value2'],skiprows=1)
pd.read_csv('test.csv',nrows=3)				#读取一定行数
```

#### 写入

```python
df1.to_csv(path_or_buf=None, sep=’,’, na_rep=”, float_format=None, columns=None, header=True, index=True, index_label=None, mode=’w’, encoding=None)
'''
path_or_buf：文件名、文件具体、相对路径、文件流等
sep：文件分割符号
na_rep：将NaN转换为特定值
columns：选择部分列写入
header：忽略列名
index：False则选择不写入索引
'''

df1.to_csv(sys.stdout,sep='-')			#写入时指定分隔符
df1.to_csv(sys.stdout,na_rep='NULL')	#将NaN转为特定字符串
df1.to_csv(sys.stdout,header=None)		#不写入列名
df1.to_csv(sys.stdout,index=False)		#不写入索引
df1.to_csv(sys.stdout,columns=['B','A'])#保留部分列且排序
```

### Excel文件

#### 读取

```python
pd.read_excel(io, sheet_name=0, header=0, skiprows=None, skip_footer=0, index_col=None, names=None, usecols=None, parse_dates=False, date_parser=None, na_values=None, thousands=None, convert_float=True, converters=None, dtype=None, true_values=None, false_values=None, engine=None, squeeze=False, **kwds)
'''
filepath_or_buffer：文件名、文件具体或相对路径、文件流（open()函数打开等）
usecols：保留指定列
sep、delimiter：俩者均为文件分割符号，或为正则表达式
header：当文件中无列名需将其设为None
names：结合header=None，读取时传入列名
skiprows：忽略特定的行数
nrows：读取一定行数
na_values：一组将其值转换为NaN的特定值
sueeze：返回Series对象
sheet_name：选择excel文件中的sheet表格，可为数值或string
'''

pd.read_excel('test4.xlsx')			#读取第一个sheet表格
#读取指定sheet表格
pd.read_excel('test4.xlsx',sheet_name=1,header=None,names=['k1','k2','value1','value2'])
```

#### 写入

```python
df1.to_excel(excel_writer, sheet_name=’Sheet1’, na_rep=”, float_format=None, columns=None, header=True, index=True, index_label=None, startrow=0, startcol=0, engine=None, merge_cells=True, encoding=None, inf_rep=’inf’, verbose=True, freeze_panes=None)
'''
excel_writer：文件名、文件具体、相对路径、文件对象等
sheet_name：写入时设定Sheetname，默认为’Sheet1’
sep：文件分割符号
na_rep：将NaN转换为特定值
columns：选择部分列写入
header：忽略列名
index：False则选择不写入索引
'''

df1.to_excel('df1.xlsx')    # 写入单个Sheet表格，只能写入一个Sheet表格，多次写入则会清除excel文件中的内容
df1.to_excel('df1.xlsx',sheet_name='df1')    # 指定Sheetname
#同一个Excel文件写入多个Sheet表格写入多个表时，需要用到pd.ExcelWriter()打开一个Excel文件
work=pd.ExcelWriter('df2.xlsx')
df1.to_excel(work,sheet_name='df2')
df1['A'].to_excel(work,sheet_name='df3')
```

## pandas下标存取

### [ ]操作符

单列标签，多列标签，行、列索引整数切片

### .loc[ ]和.iloc[ ]存取器

.loc[y]/.iloc[y]：y可以是单个值，也可是多个值（列表），y代表行索引

.loc[y，x]/.iloc[y，x]：y代表行索引，x代表列索引

```python
df1.loc['r1']						#单行
df1.loc[['r1','r2']]				#多行	
df1.loc[['r1','r2'],['c1','c3']]	#行列筛选
```

.iloc[ ]与.loc[ ]相似，但不同的是，.iloc[ ]使用**整数下标**:

### .ix[ ]存取器

.ix[ ]特点为综合了前面的，可以混用标签、位置下标存取。下标中，第一个为行索引，第二个为列索引。

```python
df1.ix[1:3,['c1','c3']]
```

### 获取单个值

.at[ ]和.iat[ ] 能使用标签和整数下标获取单个值。此外，推荐.get_value(),相比前面的更快。

### query()方法

当需要筛选时

```python
df1[(df1.c1>4)&(df1.c3<5)]
df1.query("c1>4 and c3<5")		#better
```

## 时间对象

### 时间点Timestamp

Timestamp是从Python标准库的datetime类继承过来的，表示时间轴上的一个时刻。它提供了方便的时区转换功能。

```python
now=pd.Timestamp.now()
#2019-02-25 10:36:01.338081
now_shanghai=now.tz_localize("Asia/Shanghai")		#转换为指定的时区
```

### 时间段Period

Period表示一个标准的时间段。例如某年、某月、某日、某小时等。时间的长短由freq决定。

```python
now_day=pd.Period.now(freq="H")
print(now_day)
print(now_day.start_time)
print(now_day.end_time)
#2019-02-25 10:00
#2019-02-25 10:00:00
#2019-02-25 10:59:59.999999999
```

### 时间间隔TImedetla

通过调用**pd.Timedelta()**之间创建时间间隔Timedelta对象，Timedelta对象有属性：weeks、days、seconds、milliseconds、microseconds和nanoseconds等。

```python
td=pd.Timedelta(weeks=2,days=10,hours=12,minutes=2.4,seconds=10.3)
#24 days 12:02:34.300000
```

## 时间序列

Timestamp、Period和Timedelta对象都是单个值，这些值都可以放在索引或数据中。作为索引的时间序列有：DatetimeIndex、PeriodIndex和TimedeltaIndex，它们都可以作为Series和DataFrame的索引。

## pandas其他方法

### ReIndex

reindex()作用是创建一个新索引的新对象。

```python
df1.reindex(index=['a','b','c','d'],columns=['one','two','three','four'],fill_value=100)

```

传入**method=” “**重新索引时选择插值处理方式，传入**fill_value=n**用n代替缺失值。

### Drop

丢弃某轴上项，只要有一个索引表或者列表即可。

```python
df1.drop([1,0])
df1.drop(['x','z'],axis=1)
```

### Dropna

pandas使用NaN作为缺失数据的标记。使用dropna使得滤除缺失数据更加容易。

```python
df1=pd.DataFrame([[1,2,3],[NaN,NaN,2],[NaN,NaN,NaN],[8,8,NaN]])
df1.dropna(how='all')			#清除全为NaN的行
df1.dropna(axis=1,how="all")	#滤除列
df1.dropna(thresh=1)			#滤除n行
```

### fillna

fillna()填充丢失数据。

```python
df1.fillna(100)					#使用常数
df1.fillna({0:10,1:20,2:30})	#使用字典
df1.fillna(0,inplace=True)		#直接修改原对象
df2.fillna(method='ffill')		#用前面的值来填充，使用method插值方法
df2.fillna(method='bfill',limit=2)#limit限制填充个数
df2.fillna(method="ffill",limit=1,axis=1)#axis修改填充方向
```

### 排序

根据Series对象的**索引**、**值**排序。

```python
se1=pd.Series(np.arange(10,13),index=[1,3,2])		#数字索引
se2=pd.Series(np.arange(0,3),index=['c','d','a'])	#字符索引
se2.sort_index(ascending=False)						#降序
se3=pd.Series([3,-5,7])
se3.sort_values()									#值排序
```

DataFrame排序

通过**axis参数**可以对任意轴排序

```python
df1=pd.DataFrame(np.arange(9).reshape(3,3),index=list("bac"),columns=list("yzx"))
df1.sort_index(axis=1)
df2.sort_values(by=['a','b'])
```

### 排名

排名是根据Series对象或DataFrame的某几列的值进行排名，.rank(method=，ascending=,…)返回对值的排名。但需要十分注意如何处理出现相同的值。

```python
se5=pd.Series([2,3,7,5,3,7])		#平均排名
se5.rank(method="first")			#顺序排名
se5.rank(method="min",ascending=False)#最小值排名
se5.rank(method="max",ascending=False)#最大值排名
se5.rank(method="first",ascending=False)#降序排名
```
