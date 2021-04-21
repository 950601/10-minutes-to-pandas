# 10-minutes-to-pandas

## 首先引入包

```
import numpy as np
import pandas as pd
```

### 创建数据对象
> 数据对象一般包括Series,DateFrame，详细介绍：https://pandas.pydata.org/docs/user_guide/dsintro.html#dataframe

- **创建一个series对象，使用pandas默认索引**
> s = pd.Series([1, 3, 5, np.nan, 6, 8])


- **创建一个DateFrame对象，指定日期行头与大写字母列头，使用随意数据**
```
dates = pd.date_range("20130101", periods=6)
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list("ABCD"))
```

- **创建一个DateFrame对象，使用不同的类型**
```
df2 = pd.DataFrame(
       {
           "A": 1.0,
           "B": pd.Timestamp("20130102"),
           "C": pd.Series(1, index=list(range(4)), dtype="float32"),
           "D": np.array([3] * 4, dtype="int32"),
           "E": pd.Categorical(["test", "train", "test", "train"]),
           "F": "foo",
        }
    )
```


### 查看数据 

- **查看前后数据**
```
df.head()
df.tail(3)  # default value is 5
```

- **查看列头，行头**

```
df.index
df.columns
```


- **DateFrame转NumPy Array**
> 由于DateFrame拥有多种类型，而NumPy只允许全部为一种类型，所以df2不推荐使用此方法
```
df.to_numpy()
df2.to_numpy()
```

- **快速统计**

```
df.describe()
```

- **行列调转（Transposing ）**

```
df.T
```
- **排序**
> axis = 1 代表以列排序，0 代表以行排序
> by = clounm 指定具体排序列
```
df.sort_index(axis=1, ascending=False)
df.sort_values(by="B")
```




### 选择数据

 - **获取数据**

```

```
