## 操作Excel
&emsp;&emsp;Excel是最常见的数据数理和数据存储的工具，Pythonr操作Excel的方式有很多种，常见的有：
- xlrd、xlwr：常见的，pandas也是基于此；
- [openpyxl]()：openpyxl只能操作xlsx文件而不能操作xls文件,read_only、write_only两种模式可以对大批量进行极速处理；
- [xlwings](https://www.xlwings.org/)：与pandas完美结合，并且支持替代VBA宏。  
这里暂且以pandas为例，整理日常场景中的操作。 
### 读取Excel
```python
import pandas as pd
df=pd.read_excel(r'D:\test.xlsx')  # 直接默认读取到这个Excel的第一个表单
# df=pd.read_excel(filepath,sheet_name='Sheet1')  # 可以通过sheet_name来指定读取的表单
data=df.head()  # 默认读取前5行的数据
print("获取到所有的值:\n{0}".format(data))  # 格式化输出
```
### 数据维度（行列）
```python
# 查看数据表的维度(行列)
df.shape
```

