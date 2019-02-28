## 如何节省 Notebook 运行内存

### 1. 读取数据时指定数据类型
比如某一列的数据范围肯定在0~255之中，那么我们可以指定为np.uint8类型，如果不手动指定的话默认为np.int64类型，这之间的差距巨大。对于浮点数如果不指定数据类型，默认的精度是float64，但是通常情况下float32，甚至float16的精度就已经足够了。所以在读取数据时指定数据类型可以节省大量内存空间。
例如我们用 pandas 读取一个 csv 文件，分别对比两者的内存占用。
```python
# 不指定数据类型读取
import pandas as pd
data_1 = pd.read_csv('btc.csv')

# 查看数据类型及内存占用情况
data_1.info()
```
当不指定数据类型时可以看到输出信息如下：
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3405857 entries, 0 to 3405856
Data columns (total 8 columns):
Timestamp            int64
Open                 float64
High                 float64
Low                  float64
Close                float64
Volume_(BTC)         float64
Volume_(Currency)    float64
Weighted_Price       float64
dtypes: float64(7), int64(1)
memory usage: 207.9 MB
```

```python
# 指定数据类型
data_2 = pd.read_csv('btc.csv', dtype={'open':np.float16, 'High':np.float16, 'Low':np.float16, 
                                       'Close':np.float16, 'Volume_(BTC)':np.float16, 
                                       'Volume_(Currency)':np.float16, 'Weighted_Price':np.float16})
data_2.info()
```
当指定数据类型时输出信息如下：
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3405857 entries, 0 to 3405856
Data columns (total 8 columns):
Timestamp            int64
Open                 float64
High                 float16
Low                  float16
Close                float16
Volume_(BTC)         float16
Volume_(Currency)    float16
Weighted_Price       float16
dtypes: float16(6), float64(1), int64(1)
memory usage: 90.9 MB
```

从以上对比可以看出，在读取数据时，指定合适的数据类型减少了一倍多的内存空间，是一种简单有效的方法。




尽量减少 DataFrame 的数量
尽量减少赋值导致的 COPY, 修改时带上 inplace=True

### 2. 使用多线程释放不需要的中间变量
Python 程序 在 Linux 或者 Mac 中，哪怕是 del 这个对象，Python 依旧不把内存还给系统，自己先占着直到进程销毁。

### 3. 将字符串的类别变量转换成整数
字符串占的内存比数字要大很多，所以要尽量用整数来替换字符串。比如我有一个叫“country“的列，它的值有'China', 'USA', 'Italy', 那么我可以用一个map把这三个值分别映射到1，2，3。这样也可以节省内存。

### 4. 使用生成器来读取原始数据

### 5. 图片数据集读取时只存储图片名称的列表

### 6. 训练神经网络时，减少 batch_size

内存的减少是靠牺牲时间资源或CPU资源来换取的，实际使用时要综合权衡。
