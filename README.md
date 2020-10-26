# program-in-GeoGebra
一行流

## 前言

突然想通了一些事，切换工具了。

GGB 的函数名称有中英文双版本

### 求数列的 Sn

```
f(x)=2*x+1
```

这步定义函数

```
sum(f(t),t,0,n) 
```

这步计算累和，ggb 会自动识别 n，并询问是否新建变量。

### 拓展：求解黎曼和

```
f(x)=2*x+1
```

定义函数

```
RectangleSum(f, 0, 5, 10, 0)
```

### 求解 fib 的第 n 项

```
迭代(a1 + b1, a1, b1, {1, 1}, n)
```

这里解释一下

在 Python 里，fib 数列是这么写的

```py
def fib():
    a, b = 1, 1
    for _ in range(5):
        a, b = b, a + b
    return a


print(fib())
```

注意对应关系即可

### 求任意带系数的线性递推的 n

以 a<sub>n+2</sub> = 2 a<sub>n+1</sub> + a<sub>n</sub>, a<sub>1</sub> = a<sub>2</sub> =1 为例

结合 fib 数列的思路，容易写出

```
迭代(2*b1 + a1, a1, b1, {1, 1}, n)
```

注意变量顺序

### 求解 taylor 展开

```
TaylorSeries(sin(x), 0, 5)
```

### 表示 n 进制

```
ToBase(99,2)
```

返回值是 Text

### 绘制累和函数

```
sum(abs(x - t), t, 0, 5)
```

### 绘制撞球摆

To Be Continued...

### 只用分支的方阵画法

To Be Continued...

### 变系数递推

a<sub>1</sub>=a<sub>2</sub>=1, a<sub>n</sub>=n a<sub>n-1</sub> +a<sub>n-2</sub>

对于这种迭代公式变化的，就不能用 迭代列表 轻松地解决。

下面分步处理

```
l2= {1,1}
```

初始列表

由于 GGB 不支持负数索引，所以需要用 `最后元素/Last` 提取一个子列表，再用 `元素/Element` 提取第一个

```
元素(最后元素(l2, 1), 1) 
```

这样提取了最后一个元素

那怎么处理 n 呢？注意到填充第 n 项前，列表长度 n-1

故

```
元素(最后元素(l2, 1), 1) (长度(l2) + 1)
```

这样就解决了 n a<sub>n-1</sub> 这项

另一项是常系数

```
元素(最后元素(l2, 2), 1))
```

加起来就得到了 append 的对象

```
元素(最后元素(l2, 1), 1) (长度(l2) + 1) + 元素(最后元素(l2, 2), 1))
```

把 追加/append 写进去

```
追加(l2, 元素(最后元素(l2, 1), 1) (长度(l2) + 1) + 元素(最后元素(l2, 2), 1))
```

这就是一个关于 l2 的函数，可以用迭代功能了。

#### Note

直接写

```
迭代(追加(l2, 元素(最后元素(l2, 1), 1) (长度(l2) + 1) + 元素(最后元素(l2, 2), 1)), l2, {1, 1}, 5)
```

会报错，因为 GGB 会把 l2 解析为数字。

所以要套一层 `{}`

```
迭代(追加(l2, 元素(最后元素(l2, 1), 1) (长度(l2) + 1) + 元素(最后元素(l2, 2), 1)), l2, {{1, 1}}, 5)
```

对照一下 MMA 的输出

```mma
RecurrenceTable[{a[n] == n*a[n - 1] + a[n - 2], a[1] == 1, a[2] == 1}, a[n], {n, 1, 10}]
```

> {1, 1, 4, 17, 89, 551, 3946, 32119, 293017, 2962289}

