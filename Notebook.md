## 如何节省 Notebook 运行内存

### 1. 读取数据时指定数据类型
比如某一列的数据范围肯定在0~255之中，那么我们可以指定为np.uint8类型，如果不手动指定的话默认为np.int64类型，这之间的差距巨大。
读取 CSV 的时候指定相关列的类型 {‘col_a’: np.float64, ‘col_b’: np.int32}，否则 pandas 会产生大量的 object

以numpy为例，浮点数有以下几种精度，float16, float32, float64, 并且默认的精度是float64。但是通常情况下float32，甚至float16的精度就已经足够了，所以可以用array的astype函数将float64转成float16或32。

```python
import numpy as np
type(np.random.random(5)[0])

type(np.random.random(5).astype(np.float16)[0])

```

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
