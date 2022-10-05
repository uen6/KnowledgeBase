# Matplotlib笔记总结

>   matplotlib是一个python 2D绘图库，利用它可以画出许多高质量的图像。只需几行代码即可生成直方图，条形图，饼图，散点图等。
>
>   Matplotlib可用于Python脚本，Python和IPython shell，Jupyter笔记本，Web应用程序服务器和四个图形用户界面工具包。





## Matplotlib，pyplot和pylab？

Matplotlib是整个包，pyplot是Matplotlib中的一个模块，并且pylab是一个安装在一起的模块。

pylab和pyplot的区别是，前者将numpy导入了其命名空间中，这样会使pylab表现的和matlab更加相似。现在来说我们经常使用pyplot，因为pyplot相比pylab更加纯粹。





## 安装

```python
pip install matplotlib
```

使用国内镜像安装：

```python
pip install matplotlib -i https://pypi.tuna.tsinghua.edu.cn/simple
```

**清华：**`https://pypi.tuna.tsinghua.edu.cn/simple`
**阿里云：**`http://mirrors.aliyun.com/pypi/simple/`
**中国科技大学：**`https://pypi.mirrors.ustc.edu.cn/simple/`
**华中理工大学：**`http://pypi.hustunique.com/`
**山东理工大学：**`http://pypi.sdutlinux.org/`
**豆瓣：**`http://pypi.douban.com/simple/`





## Matplotlib基础功能

```python
import matplotlib.pyplot as plt
# ...此处对图进行设置
plt.show()
```

### 调整标题

```python
plt.title("title")  # 括号当中输入标题的名称
# 如果title是中文，matplotlib会乱码，这时需要加上下面这段代码：
# plt.rcParams['font.sans-serif']=['SimHei']
```

### 调整x、y轴和标签

```python
plt.xlim(0, 6)  # x轴的上下限
plt.ylim((0, 3))  # y轴的上下限
plt.xlabel('X')  # x轴标签
plt.ylabel('Y')  # y轴标签
# 如果需要将数字设为负数，也可能出现乱码的情况，这时候可以加下面的代码：
# plt.rcParams['axes.unicode_minus']=False
```

### 调整label和legend

设置label和legend的目的就是为了区分出每个数据对应的图形名称，legend的loc参数用于设置图例位置

```python
# plt.plot()为绘图函数
plt.plot(2, 3, label="one")  # 第一个label
plt.plot(2, 3 * 2, label="two")  # 第二个label
plt.legend(loc='best')  # 图列位置，可选best，center等
```

### 调整图的尺寸

在matplotlib中，整个图像为一个Figure对象。在Figure对象中可以包含一个或者多个Axes对象。每个Axes(ax)对象都是一个拥有自己坐标系统的**绘图区域**。

```python
plt.figure(figsize=(6, 3))  # 图像大小 6*3
```

### 增加注释（标注某个点）

```python
# s: 注释信息内容
# xy: 箭头点所在的坐标位置
# xytext: 注释内容的坐标位置
# arrowprops: 设置指向箭头的参数
plt.annotate(s='标记点', xy=(3, 2), xytext=(4, -0.5), weight='bold', color='b',
             \arrowprops = dict(arrowstyle='-|>', color='k'))
```

### 保存图片

```python
plt.savefig()  # 保存
```





## plot()函数的格式控制

| 颜色(color='...')                            | 线形状('...')  | 点形状(marker='...')                                         |
| -------------------------------------------- | -------------- | ------------------------------------------------------------ |
| `r`、`y`、`b`、`g`<br />（红、黄、蓝、绿色） | `:`（点线）    | `V`、`>`、`<`、`^`（不同方向的尖三角）                       |
| `w`、`k`（白、黑）                           | `-.`（点画线） | `s`、`d`、`p`、`h`、`o` <br />（正方形、菱形、五边形、六边形、圆形） |
| `c`、`m`（青、洋红）                         | `--`（短划线） | `1`、`2`、`3`、`4`（不同方向的Y形标志）                      |
| `#ffec88`                                    | `-`（实线）    | `+`、`-`、`|`、`.`、`,`（对应形状的标志）                    |

**提示：**三种样式可以组合写

```python
plt.plot(y, 'gx:')
plt.plot(y + 1, 'mo--')
plt.plot(y + 2, 'bp-')
```





## 图表类型

### 柱状图

```python
plt.bar(x, y)
```

### 多系列柱状图

```python
# kind: 可选类型有线形图、柱状图、密度图、堆叠图、面积图等
# grid: 显示网格
# colormap: 颜色展示
# stacked: 堆叠效果，多个柱状图叠在一起

plt.plot(kind='bar', grid=True, colormap='summer_r', stacked=true)
```

### 水平条形图

```python
plt.barh(grid=True, colormap='BuGn_r', stacked=true)
```

### 散点图

```python
# c: 颜色
# s: 点的大小，默认20
# marker: 点的形状，默认圆形
# alpha: 指定对象的透明度

plt.scatter(x, y)
plt.scatter(x, y, s, c=colors, alpha=0.5)
```

### 饼图

```python
# labels: 标签
# autopct: 控制饼图内百分比设置
# shadow: 阴影，False即不画

plt.pie(1, 10, 3, ...)  # 必须输入的参数是饼图每个部分的值

sizes = [2, 5, 12]
labels = ['娱乐', '育儿', '饮食']
plt.pie(sizes, labels=labels, autopct='%1.1f%%', shadow=False, startangle=100)
```

### 直方图

```python
# bins: 直方图的长条形数目，可选项，默认为10
# normed: 是否将得到的直方图向量归一化，可选项，默认为0，代表不归一化，显示频数。normed=1，表示归一化，显示频率。
# facecolor: 长条形的颜色
# edgecolor: 长条形边框的颜色
# alpha: 透明度
# stacked: 堆叠效果

plt.hist(bins=20, histtype='bar', align='mid', orientation='vertical', alpha=0.5, normed=True)
```

### 极坐标图

```python
# theta: 角度数据
# radii: 极径数据
# theta_direction: 设置极坐标的正方向
# theta_zero_location: 方法用于设置极坐标0°位置，0°可设置在八个位置，分别为N, NW, W, SW, S, SE, E, NE
# thetagrids: 方法用于设置极坐标角度网格线显示
# theta_offset: 方法用于设置角度偏离

plt.subplot(121, projection='polar')
plt.scatter(theta, r, c=colors, s=area, cmap='hsv', alpha=0.75)
```

### 其他图表

| 类型     | 对应函数   |
| -------- | ---------- |
| 箱型图   | boxplot()  |
| 热图     | heatmap()  |
| 量场图   | quiver()   |
| 灰度图   | imshow()   |
| 等高线图 | contourf() |



