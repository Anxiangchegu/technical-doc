## 核心数据结构
##### pandas最核心的就是Series和DataFrame两个数据结构。     

类型 | 维度 | 说明  
---| --- | ---
Series | 1维 | 带有标签的同构类型数组
DataFrame | 2维 | 二维标签，大小可变的表格结构与潜在的异型列
Panel | 3维 | Panel 是 DataFrame 的容器 

DataFrame可以看做是Series的容器，即：一个DataFrame中可以包含若干个Series。  

## 创建数据表
最常见的数据类型是二维的 DataFrame，其中：
- 每行代表一个示例 (instance)
- 每列代表一个特征 (feature)
DataFrame 可理解成是 Series 的容器，每一列都是一个 Series，或者Series 是只有一列的 DataFrame。
Panel 可理解成是 DataFrame 的容器。

### 一维 Series
```python
# 创建 Series
pd.Series( x, index=idx )
```

其中 x 可以是：列表 (list)、numpy 数组 (ndarray)、字典 (dict)
#### 1、用列表创建
```python
import pandas as pd
# 用列表
s = pd.Series([27.2, 19, 27.8, 25])
print("打印元素：", s.values)
print("打印元素对应的索引：", s.index)
dates = pd.date_range('20190401', periods=4)
s2 = pd.Series([27.2, 19, 27.8, 25], index=dates)
print(s2)
print("打印元素对应的索引：", s2.index)
s2.name = '温度'
print(s2)
```
#### 2、用数组创建
```python
# 使用数组生成Series，加入缺失值 np.nan
s3 = pd.Series(np.array([27.2, 27.65, 27.70, 28, 28, np.nan]))
print('长度：', len(s3))
print('形状：', s3.shape)
print("不含 nan 的元素个数：", s3.count())
print("非 nan 元素的出现次数：", s3.value_counts())
print("不重复的元素：", s3.unique())
```

#### 3、用字典创建

```python
# 字典的「键值对」的「键」自动变成了 Series 的索引 (index)，而「值」自动变成了Series 的值 (values)。
data_dict1 = {'Name': 'Zara', 'Age': 7, 'Class': 'First'}
data_dict2 = {'Address': 'SiChuan', 'hobby': 'swimming'}
s4 = pd.Series(data_dict1, name="Zara的信息")
s4.index.name = "属性"
print(s4)
```

### 二维 DataFrame

```python
# 创建DataFrame
pd.DataFrame( x, index=idx,columns=col )
```
- 其中 x 可以是：
    - 二维列表 (list)
    - 二维 numpy 数组 (ndarray)
    - 字典 (dict)，其值是一维列表、numpy 数组或 Series
    - 另外一个 DataFrame
    
#### 1、用列表或 numpy 数组
```python
df1 = pd.DataFrame([[1, 2, 3], [4, 5, 6]])
df1 = pd.DataFrame(np.array([[1, 2, 3], [4, 5, 6]]))
```
- 升维
用 MultiIndex.from_tuples() 还可以赋予 DataFrame 多层索引 (实际上增加了维度，多层索引的 DataFrame 实际上是三维数据)。
```python
df2.index = pd.MultiIndex.from_tuples( 
            [('中国公司','BABA'), ('中国公司','JD'), 
             ('美国公司','AAPL'), ('美国公司','MS'), 
             ('美国公司','GS'), ('美国公司','WMT')] )
```
### 三维Panel
`不推荐使用Panel，将在将来的版本中删除。`  
&emsp;&emsp;这些类型的三维数据的推荐方法是通过Panel.to_frame()方法在数据aframe上使用多索引
或者，您 可以使用xarray包http://xarray.pydata.org/en/stable/。
panda提供了一个' .to_xarray() '方法来帮助自动转换。

## 数据类型
#### 主要数据类型

类型 | 维度
---|---
int | 整数型
float | 浮点数
object | Python对象类型，用O表示
string | 字符串类型，经常用S表示，S10表示长度为10的字符串
unicode_ | 固定长度的unicode类型，跟字符串定义方法一致
datetime64[ns] | 表示时间格式

## 常用函数

函数名 | 说明
---|---
astype() | 获取某一列的数据类型
dtype() | 转换数据类型
set_index() | 重新设置索引列
reset_index() | 重置索引，经常用于数据分组、透视表中
rename() | 重命名行索引或列索引
replace() | 替换，可以实现多对多替换
sort_value() | 排序
drop() | 删除某列
value_counts() | 计数
drop_duplicates | 返回删除重复行的DataFrame
isin() | 查找是否包含
cut() | 区间切分，结果是左开右闭的区间
qcut() | 指明切分个数进行区间切分
insert() | 插入新的列（Python中没有直接插入行的方法）
.T | 行列互换
stack() | 将表格型数据转化为树形数据
unstack() | 将树形数据转换为表格形数据
melt() | 宽表转换为长表
map() | 对一个序列中的所有元素执行相同的函数操作
apply() | 对DataFrame中的某一列或行中的元素执行相同的函数操作，可以配合lambda结合使用
applymap() | 对DataFrame中的每一个元素执行相同的函数操作
count() | 数据表中每列的非空值的个数
sum() | 可以对每一列或行进行求和，也可以单独求和
mean() | 求均值，与sum()用法类似
max() | 求最大值
min() | 求最小值
median() | 求中位数
mode() | 求众数
var() | 求方差
std() | 求标准差
quantile | 求分位数
correl() | 相关性运算

+ 示例

```python
# 查看数据集是否存在缺失值
df.apply(lambda x:np.sum(x.isnull()))

# 数值型变量的统计描述:个数(count）、平均值（mean）、标准差（std）、最小值（min）、下四分位数（25%）、中位数（50%）、上四分位数（75%）和最大值（max）。
df.describe()

# 离散型变量的统计描述：非缺失观测的数量（count）、不同离散值的个数（unique）、出现频次最高的离散值（top）和最高频次数（freq）。
df.describe(include =[ 'object'])
```

##### 时间序列

+ from datetime import datetime

函数名 | 说明
---|---
now() | 当前时间
now().year | 年  
now().month | 月  
now().day | 日  
now.weekday() + 1 | 周几
now.isocalendar() | 返回周数  
now().date() | 当前日期
now().time() | 当时时间
now().strftime | 自定义日期和时间格式  
str() | 将日期格式转化为字符串格式  
parse() | 将字符串转化为时间格式 

+ 示例

```python
datetime.now().strftime("%Y-%m-%d %H:%M:%S")
pd.to_datetime(pd.Series(['05/23/2019']), format="%m/%d/%Y")  # 2019-05-23
```

##### 数据分组
函数名 | 说明
---|---
groupby() | 数据分组
aggregate() | 多种汇总方式  
pivot_table() | 数据透视表

##### 多表拼接 
函数名 | 说明
---|---
merge() | 默认寻找两个表的公共列进行连接
merge(df1, df2, on = 'id') |  
concat() | 纵向连接

- 数据表的合并与连接  
&emsp;&emsp;数据表可以按「键」合并，用 merge 函数；可以按「轴」来连接，用 concat 函数。单键合并用 merge 函数，语法如下：  
    pd.merge( df1, df2, how=s, on=c )  
c 是 df1 和 df2 共有的一栏，合并方式 (how=s) 有四种：  
   - 左连接 (left join)：合并之后显示 df1 的所有行  
   - 右连接 (right join)：合并之后显示 df2 的所有行  
   - 外连接 (outer join)：合并 df1 和 df2 共有的所有行  
   - 内连接 (inner join)：合并所有行 (默认情况)  

- 单键合并

```python
df_price = pd.DataFrame( {'Date': pd.date_range('2019-1-1', periods=4),'Adj Close': [24.42, 25.00, 25.25, 25.64]})
df_volume = pd.DataFrame( {'Date': pd.date_range('2019-1-2', periods=5),
    'Volume' : [56081400, 99455500, 83028700, 100234000, 73829000]})
    
# left join
pd.merge( df_price, df_volume, how='left')

# outer join
pd.merge( df_price, df_volume, how='outer')

```

- 多键合并

```python
porfolio1 = pd.DataFrame({'Asset': ['FX', 'FX', 'IR'], 'Instrument': ['Option', 'Swap', 'Option'], 'Number': [1, 2, 3]})
porfolio2 = pd.DataFrame({'Asset': ['FX', 'FX', 'FX', 'IR'], 'Instrument': ['Option', 'Option', 'Swap', 'Swap'], 'Number': [4, 5, 6, 7]})
# 在 'Asset' 和 'Instrument' 两个键上做外合并。
pd.merge( porfolio1, porfolio2, 
          on=['Asset','Instrument'],
          how='outer')
```

- 连接DataFrame

```python
import numpy as np
import pandas as pd

# 沿着行连接 (axis = 0)
df1 = pd.DataFrame(np.arange(12).reshape(3, 4), columns=['a', 'b', 'c', 'd'])
df2 = pd.DataFrame(np.arange(6).reshape(2, 3), columns=['b', 'd', 'a'])

# 沿着行连接分两步:先把 df1 和 df2 列标签补齐,再把 df1 和 df2 纵向连起来
print(pd.concat([df1, df2], sort=True))
# 得到的 DataFrame 的 index = [0,1,2,0,1]，有重复值。如果 index 不包含重要信息 (如上例)，可以将 ignore_index 设置为 True，这样就得到默认的 index 值了。
print(pd.concat([df1, df2], ignore_index=True, sort=True))

print("***************列连接*****************")
# 沿着列连接 (axis = 1)
df3 = pd.DataFrame(np.arange(6).reshape(3, 2), index=['a', 'b', 'c'], columns=['one', 'two'])
df4 = pd.DataFrame(5 + np.arange(4).reshape(2, 2), index=['a', 'c'], columns=['three', 'four'])

# 沿着列连接分两步：先把 df1 和 df2 行标签补齐，再把 df1 和 df2 横向连起来
print(pd.concat([df3, df4], axis=1, sort=True))
```
