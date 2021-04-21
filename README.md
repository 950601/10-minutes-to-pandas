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


### 查阅数据 

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




### 获取数据


- **根据名称和位置获取**

> 获取A列数据，返回DataFrame类型，索引（index）为日期
```
df["A"] / df.A
```
> 获取0-3行数据
```
df[0:3]
```
> 获取20130102-20130104行数据
```
df["20130102":"20130104"]
```
> 获取第3行数据
```
df.iloc[3]
```
> 获取第3-5行，0-2列数据
```
df.iloc[3:5, 0:2]
```
> 获取第1,2,4行，0-2列数据
```
df.iloc[[1, 2, 4], [0, 2]]
```
> 获取全部行，全部列
```
df.iloc[:, 1:3]
```
> 获取单个数据，高精度
```
df.iloc[1, 1]
```



- **多形式获取数据**
> 获取index = dates[0]的数据，返回DataFrame类型，索引为列头（A,B,C,D）
```
df.loc[dates[0]]
```

> 获取所有行，以及A,B列
```
df.loc[:, ["A", "B"]]
```

> 获取指定范围内的行与列
```
df.loc["20130102":"20130104", ["A", "B"]]
```

- **根据条件获取数据**
> 获取A列大于0的数据
```
df[df["A"] > 0]
```

> 获取DateFrame大于0的数据，小于0返回none
```
df[df > 0]
```

> 根据是否存在指定value获取数据
```
df2[df2["A"].isin(["0.469112", "1.212112"])]
```


- **设置获取数据**
> 在已有数据上加一列，根据index匹配行
```
s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range("20130102", periods=6))
df["F"] = s1
```

> 在指定位置设置value
```
df.at[dates[0], "A"] = 0
df.iat[0, 1] = 0
df.loc[:, "D"] = np.array([5] * len(df))
```

> 根据条件设置value
```
df2 = df.copy()
df2[df2 > 0] = -df2
```


### 数据过滤

- **增删改查**

> 重构DateFrame副本，reindex返回一个新的copy
```
df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ["E"])
df1.loc[dates[0] : dates[1], "E"] = 1
```

> 删除空行
```
df1.dropna(how="any")
```

> 填充空行
```
df1.fillna(value=5)
```

> 查看空行。由于副本不具备方法，所以使用原始DareFrame对象
```
pd.isna(df1)
```

### 数据操作

- **数据统计**

> 获取最小值，0代表列，1代表行
```
df.mean()
df.mean(1)
```

