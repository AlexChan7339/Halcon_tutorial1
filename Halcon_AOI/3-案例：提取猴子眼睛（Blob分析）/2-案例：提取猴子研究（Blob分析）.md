2-案例：提取猴子研究（Blob分析）

# step1：提取图像

```c
* 提取 猴子图像
read_image (Image, 'monkey')
```

monkey 图片为Halcon 自带的案例，只用输入相对路径

> 观察猴子眼睛的灰度值，都在130 以上（大部分都是白色）<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508122220660.png" alt="image-20250812222059388" style="zoom:33%;" />

# step2：阈值分割

```c
* 阈值分割
threshold (Image, Region, 130, 255)
```

**Params**:

- Image：输入图像
- Region：输出图像
- 130：灰度值下限
- 255：灰度值上限

**输出图像**

<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508122226300.png" alt="image-20250812222600197" style="zoom:50%;" />

> 我们能看到眼睛中间有黑色区域（眼球，其灰度值小于130），我们后面要提取完整的眼睛，需要对区域进行填充

# step3：填充

```c
* 填充
fill_up (Region, RegionFillUp)
```

对孔洞进行填充

**输出图像**

<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508122229280.png" alt="image-20250812222903203" style="zoom:50%;" />

# step4：打散

> 发现图上有很多散点或小区域(都称为不连通的区域，像素点之间没有点连接和线连接)，我们将其用不同的颜色对其进行区分

```c
* 打散：把不连通的给打散，用不同的颜色进行区分
connection (RegionFillUp, ConnectedRegions)
```

**打散前**

<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508122234138.png" alt="image-20250812223431013" style="zoom:50%;" />

**打散后**

![image-20250812223504480](https://typora3.oss-cn-shanghai.aliyuncs.com/202508142008850.png)

> 如果不打算，则是对所有红色区域计算总像素总个数（数值）
>
> 操作：右击图片区域--工具--特征检查--勾选`area`
>
> <img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508142045326.png" alt="image-20250814204536163" style="zoom:50%;" />

# step5:筛选

通过点击猴子的左眼和右眼，能发现左眼的area为869，右眼的area为954因此设置area的范围为【800， 100】，右击`图像变量`点击清除\显示，得到输出图像

```c
select_shape (ConnectRegions, SelectRegions, area, 'and', 800, 1000)
```

**输出图像**

<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508142101633.png" alt="image-20250814210122548" style="zoom:50%;" />

# step6:腐蚀

> 我们看到右眼筛选后的区域里有毛刺，需要用腐蚀算子对其进行操作
>
> ![屏幕截图 2025-08-14 210210](https://typora3.oss-cn-shanghai.aliyuncs.com/202508142103084.png)

## 原理

如果每个红色元素，其周围圆领域内（R=3.5）的像素都是红色，则该像素保留，否则将其腐蚀掉（去杂质）

## 代码

```c
erosion_circle (SelectedRegions, RegionErosion, 3.5)
```

> 半径一般取值为.5 这种小数，这样的话半径3.5 正好是半径区域为3个半的像素块

# step7：膨胀

## 原理

不改变原来的提取区域（腐蚀和膨胀一般都是一起使用的）

## 代码

```c
dilation_circle (RegionErosion, RegionDilation, 3.5)
```

> 边缘向外扩张3个像素块

<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508231544520.png" alt="image-20250823154339451" style="zoom:50%;" />

> 从图像变量信息中可以看出这两个眼球是两个而不同的变量，接下来我们需要将两个眼球合并为一个变量（用一个颜色来表示）
>
> <img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508231551076.png" alt="image-20250823155034527" style="zoom:50%;" />

# step8:合并

## 代码

```c
union1 (RegionDilation, RegionUnion)
```

<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508231547422.png" alt="image-20250823154657786" style="zoom:50%;" />

# 总结

Blob 分析流程：

1. 读取图像
2. 阈值分割
3. 填充
4. 打散
5. 筛选
6. 形态学操作
   1. 腐蚀
   2. 膨胀
