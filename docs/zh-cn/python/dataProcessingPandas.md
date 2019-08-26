## 操作Excel
&emsp;&emsp;Excel是最常见的数据数理和数据存储的工具，Pythonr操作Excel的方式有很多种，常见的有：
- xlrd、xlwr：常见的，pandas也是基于此；
- [openpyxl]()：openpyxl只能操作xlsx文件而不能操作xls文件,read_only、write_only两种模式可以对大批量进行极速处理；
- [xlwings](https://www.xlwings.org/)：与pandas完美结合，并且支持替代VBA宏。  
这里暂且以pandas为例，整理日常场景中的操作。 

#### 读取
```python
import pandas as pd

# Pandas读取Excel
df=pd.read_excel(r'D:\test.xlsx')  # 直接默认读取到这个Excel的第一个表单
df=pd.read_excel(filepath,sheet_name='Sheet1')  # 可以通过sheet_name来指定读取的表单
# header参数值默认为0，即用第一行作为列索引；usecols表示要导入第几列
data=df.head()  # 默认读取前5行的数据
print("获取到所有的值:\n{0}".format(data))  # 格式化输出

# Pandas读CSV
pd.DataFrame.from_csv("csv_file") 
# read_csv()默认文件中的数据都是以逗号分开，也可以用sep=""指定分隔符；nrows指定前几行
pd.read_csv("csv_file")

# Pandas读取txt文件
pd.read_table(r"c:\data\test.txt", sep=" ")  # 须用sep指明分隔符

```

#### 数据写入
```python
# 输出到Excel格式
df.to_Excel('Excel_to_Python.xlsx', sheet_name='bluewhale_cc')

# 输出到CSV格式
df.to_csv("data.csv", sep=",", index=False) # 逗号分隔，没有下标
```

#### DataFrame 数据的保存和读取
- df.to_csv 写入到 csv 文件
- pd.read_csv 读取 csv 文件
- df.to_json 写入到 json 文件
- pd.read_json 读取 json 文件
- df.to_html 写入到 html 文件
- pd.read_html 读取 html 文件
- df.to_excel 写入到 excel 文件

## 检查
- 数据表

```python
data = {"id": [1001, 1002, 1003, 1004, 1005, 1006],
        "date": pd.date_range('20130102', periods=6),
        "city": ['Beijing ', 'SH', ' guangzhou ', 'Shenzhen', 'shanghai', 'BEIJING '],
        "age": [23, 44, 54, 32, 34, 32],
        "category": ['100-A', '100-B', '110-A', '110-C', '210-A', '130-F'],
        "price": [1200, np.nan, 2133, 5433, np.nan, 4432]}

columns = ['id', 'date', 'city', 'category', 'age', 'price']
labels = ['a', 'b', 'c', 'd', 'e', 'f']
df = pd.DataFrame(data, index=labels, columns=columns)
print(df)
# 选取第一行 By 整数索引切片：前闭后开
print(df[0:1])
# 选取第一行 By 标签索引切片：前闭后闭
print(df[:'a'])
```
#### 数据维度（行列）
```python
# 查看数据表的维度(行列)
df.shape
# 所有字段名
df.columns
```

#### 数据集特征
```python
# 可查看表的列名、数据类型等
df.info()
# 基本数据统计
df.describe()
```

#### 查看数据格式
```python
# 查看数据表各列格式
df.dtypes

# 查看单列数据格式
df['B'].dtype
```
#### 查看缺失值、空值
```python
#　查看数据空值
df.isnull()

#检查特定'name'列空值
df['name'].isnull()
```
#### 查看唯一值
```python
#查看city列中的唯一值
df['city'].unique()
```
#### 查看数据表数值
```python
# 查看数据表的值
df.values
```
#### 查看前、后N行
```python
df.head(n) # 前n行
df.tail(n) # 后n行
```
#### 通过特征、位置定位数据
```python
df.loc[feature_name]

# 选择“id”列的第一行
df.loc([0], ['id'])

df.iloc[n]  # 位置
```
#### loc函数查看
&emsp;&emsp;loc函数主要通过行标签索引行数据
```python
# 选择“size”列的第一行
df.loc([0], ['size'])
# 将年龄为23的修改为18
df.loc[df['age'] == 23,'age'] = 18
# 年龄>30
print(df.loc[df['age']>30,:])
# 所有的id和姓名
print(df.loc[:,('id','age')])
print(df.loc[:,['id','age']])
# 年龄大于30的id和年龄
print(df.loc[df['age']>30,['id','age']])
# 年龄等于 23 或 34的id、city、age
print(df.loc[(df['age'] == 18) | (df['age'] == 34),['id','city','age']])
```
#### iloc函数查看
&emsp;&emsp;iloc 主要是通过行号获取行数据
```python
# 选取2列
print(df.iloc[:, 1])
# 选取前3列
print(df.iloc[:, 0: 3])
# 选取第1、3、4列
print(df.iloc[:,[0,2,3]])
# 选取前3行的前3列
print(df.iloc[:3, :3])
```

#### 查看数据表统计
```python
df.describe()
```

## 清洗
#### 处理空值
```python
# 删除数据表中含有空值的行
df.dropna(how='any')

# 使用数字0填充数据表中空值
df.fillna(value=0)

# 使用price均值对NA进行填充
df['price'].fillna(df['price'].mean())
```
#### 清理空格
```python
# 清除city字段中的字符空格
df['city']=df['city'].map(str.strip)
```
#### 大小写转换
```python
# city列大小写转换,PPER，LOWER等函数
df['city']=df['city'].str.lower()
```
#### 更改数据格式
```python
# Python中dtype是查看数据格式的函数，与之对应的是astype函数，用来更改数据格式。
df['price'].astype('int')
```
#### 更改列名称
```python
# 更改列名称,category列更改为category-size。
df.rename(columns={'category': 'category-size'})
```
#### 删除重复值
```python
# Python中使用drop_duplicates函数删除重复值。
df['city'].drop_duplicates()

# 默认情况下drop_duplicates()将删除后出现的重复值(与Excel逻辑一致)。增加keep='last'参数后将删除最先出现的重复值，保留最后的值。
df['city'].drop_duplicates(keep='last')
```
#### 删除字段
```python
df.drop('city', axis=1)
# 轴对于行是0，对于列是1
```

#### 修改数值
```python
# Python中使用replace函数实现数据替换。
df['city'].replace('sh', 'shanghai')
```

## 预处理
#### 数据表合并
```python
# 在Python中可以通过merge函数一次性实现。
'''
使用merge函数对两个数据表进行合并，合并的方式为inner，将
两个数据表中共有的数据匹配到一起生成新的数据表。并命名为
df_inner。
'''
#数据表匹配合并
df_inner=pd.merge(df,df1,how='inner')
# 除了inner方式以外，合并的方式还有left，right和outer方式。
```
#### 设置索引列
&emsp;&emsp;索引列的功能很多，可以进行数据提取，汇总，也可以进行数据筛选等。设置索引的函数为set_index。
```python
# 设置索引列
df_inner.set_index('id')
```
#### 排序(按索引，按数值)
```python
# 按特定列的值排序
df_inner.sort_values(by=['age'])

# 按索引列排序
df_inner.sort_index()
```
#### 数据分组
```python
# Where函数用来对数据进行判断和分组，
# 如果price列的值>3000，group列显示high，否则显示low
df_inner['group'] = np.where(df_inner['price'] > 3000,'high','low')

# 对复合多个条件的数据进行分组标记
df_inner.loc[(df_inner['city'] == 'beijing') & (df_inner['price'>= 4000), 'sign']=1
```
#### 数据分列
```python
# 对category字段的值依次进行分列，并创建数据表，索引值为df_inner的索引列，列名称为category和size
pd.DataFrame((x.split('-') for x in df_inner['category']),index=df_inner.index,columns=['category','size'])

# 将完成分列后的数据表与原df_inner数据表进行匹配
df_inner=pd.merge(df_inner,split,right_index=True, left_index=True)
```
#### 多列合并
```python
df['省市区'] = df['省'] +  df['市'] + df['区']
# 若某一列是非str类型的数据，那么我们需要用到map(str)将那一列数据类型做转换
```
#### 将对象类型转换为数值
```python
pd.to_numeric(df["feature_name"], errors='coerce')
# 将对象类型转换为numeric以便能够执行计算(如果它们是字符串)
```

## 提取
&emsp;&emsp;loc函数按标签值进行提取，iloc函数按位置进行提取，ix函数可以同时按标签和位置进行提取。
#### 按标签提取
```python
# 按索引提取单行的数值
df_inner.loc[3]

# 按索引提取区域行数值
df_inner.loc[0:5]

# Reset_index函数用于恢复索引，这里我们重新将date字段的日期设置为数据表的索引，并按日期进行数据提取。
df_inner.reset_index()
# 设置日期为索引
df_inner=df_inner.set_index('date')

# 提取4日之前的所有数据
df_inner[:'2013-01-04']
```
#### 按位置提取（iloc）
```python
# 使用iloc按位置区域提取数据
df_inner.iloc[:3,:2]

# 使用iloc按位置单独提取数据
df_inner.iloc[[0,2,5],[4,5]]
```
#### 按标签和位置提取（ix）
&emsp;&emsp;ix是loc和iloc的混合，既能按索引标签提取，也能按位置进行数据提取。
```python
# 使用ix按索引标签和位置混合提取数据
df_inner.ix[:'2013-01-03',:4]
```
#### 按条件提取（区域和条件值）
```python
# 判断city列的值是否为beijing
df_inner['city'].isin(['beijing'])

# 先判断city列里是否包含beijing和shanghai，然后将复合条件的数据提取出来。
df_inner.loc[df_inner['city'].isin(['beijing','shanghai'])]
```

## 筛选
&emsp;&emsp;使用与，或，非三个条件配合大于，小于和等于对数据进行筛选，并进行计数和求和。
```python
# 使用“与”条件进行筛选，条件是年龄大于25岁，并且城市为beijing。
df_inner.loc[(df_inner['age'] > 25) & (df_inner['city'] == 'beijing'), ['id','city','age','category','gender']]

# 使用“或”条件进行筛选，年龄大于25岁或城市为beijing。
df_inner.loc[(df_inner['age'] > 25) | (df_inner['city'] == 'beijing'), ['id','city','age','category','gender']].sort(['age'])

# 按筛选后的结果将price字段值进行求和.
df_inner.loc[(df_inner['age'] > 25) | (df_inner['city'] == 'beijing'), ['id','city','age','category','gender','price']].sort(['age']).price.sum()

# 使用“非”条件进行筛选，城市不等于beijing。
df_inner.loc[(df_inner['city'] != 'beijing'), ['id','city','age','category','gender']].sort(['id'])

# 对筛选后的数据按city列进行计数
df_inner.loc[(df_inner['city'] != 'beijing'), ['id','city','age','category','gender']].sort(['id']).city.count()

# 使用query函数进行筛选
df_inner.query('city == ["beijing", "shanghai"]')

# 对筛选后的结果按price进行求和
df_inner.query('city == ["beijing", "shanghai"]').price.sum()
```

## 汇总
#### 分类汇总
```python
# 对所有列进行计数汇总
df_inner.groupby('city').count()

# 对特定的ID列进行计数汇总
df_inner.groupby('city')['id'].count()

# 对两个字段进行汇总计数
df_inner.groupby(['city','size'])['id'].count()

# 对city字段进行汇总并计算price的合计和均值。
df_inner.groupby('city')['price'].agg([len,np.sum, np.mean])
```
#### 透视列
```python
"""
设定city为行字段，size为列字段，price为值字段。
分别计算price的数量和金额并且按行与列进行汇总。
"""
# 数据透视表
pd.pivot_table(df_inner,index=["city"],values=["price"],columns=["size"],aggfunc=[len,np.sum],fill_value=0,margins=True)
```

## 统计
#### 数据采样
```python
# 简单的数据采样
df_inner.sample(n=3)

#手动设置采样权重
weights = [0, 0, 0, 0, 0.5, 0.5]
df_inner.sample(n=2, weights=weights)

# 采样后不放回
df_inner.sample(n=6, replace=False)

# 采样后放回
df_inner.sample(n=6, replace=True)
```
#### 描述统计
```python
# 数据表描述性统计
df_inner.describe().round(2).T
```
#### 标准差
```python
# 标准差
df_inner['price'].std()
```
#### 协方差
```python
# 两个字段间的协方差
df_inner['price'].cov(df_inner['m-point'])

# 数据表中所有字段间的协方差
df_inner.cov()
```
#### 相关分析
&emsp;&emsp;Corr函数用来计算数据间的相关系数，可以单独对特定数据进行计算，也可以对整个数据表中各个列进行计算。
```python
# 相关性分析
df_inner['price'].corr(df_inner['m-point'])

# 数据表相关性分析
df_inner.corr()
```
