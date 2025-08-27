# Halcon语法基础

> Halcon 有两大主要参数类型，一个是**图表类型**，一个是**元组类型**

# 1 元组类型

## 1.1  整数

整数：不带小数点的变量

```c#
a := 1
```

- `：= ` ：赋值操作；
- 按`F6`进行单行运行

## 1.2 浮点数

浮点数：

```c
b := 1.2
```

## 1.3 字符串类型

用**单引号**包含住的字符，字符包括数字、英文和中文

# 2 图像类型

> 包括图像、区域和轮廓

## 2.1  图像

算子、方法和函数是一个意思，这三者都有输入参数、输出参数（也就是上面的变量`a`、`b`、`c`）

### 例子

```
read_image (Image, 'printer_chip/printer_chip_01')
read_image (Image, 'D:/CALB/软件&算法&读书/Halcon/1-安装&语法基础/1.bmp')
```

- read_image:算子、函数、方法

- Image:输出参数，为图像变量

- `'D:/CALB/软件&算法&读书/Halcon/1-安装&语法基础/1.bmp'` & `'printer_chip/printer_chip_01'`:输入参数，为string类型（用单引号表示），里面的内容为路径。一般来说用绝对路线。

  - 'printer_chip/printer_chip_01'：Halcon自带的`'C:\Users\sejje\AppData\Roaming\MVTec\HALCON-25.05-Progress\examples\images\printer_chip\printer_chip_01.png'`，对Halcon来说是相对路径
  - 读取图像时，也可以将图像拖入程序窗口，此时路径则为全部路径`'D:/CALB/软件&算法&读书/Halcon/1-安装&语法基础/1.bmp'`

- 运行后，可以按住`Ctrl`放大图区域，可以看到每个像素的灰度和位置

  <img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508112129401.png" alt="image-20250811212905200" style="zoom: 50%;" />

## 2.2 区域类型

### 例子

```c
get_domain (Image, Domain)
```

<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508112123929.png" alt="image-20250811212318801" style="zoom:33%;" />

获取一块纯色区域，按住Ctrl键能看到各个像素的灰度值和位置信息

## 2.3  轮廓类型

只显示边缘轮廓线

```c
gen_contour_region_xld (Domain, Contours, 'border')
```

运行后，对`图像变量`中的`Contours`右击选择`清除\显示`,可以看到轮廓

<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508112135861.png" alt="image-20250811213522724" style="zoom:50%;" />

# 3

<img src="https://typora3.oss-cn-shanghai.aliyuncs.com/202508122203430.png" alt="在这里插入图片描述" style="zoom:50%;" />

## 3.1 Tab 键

自动不全代码。当我们输入不全的算子名称，可以多按几次Tab键，可以实现整行代码的补全，直到出现算子的`）`

## 3.2  单步运行：F5

逐步运行代码

## 3.3 运行所有：F6

一次性运行所有代码

## 3.4 重置运行：F2

重新回到代码的第一行，重新开始运行代码

## 3.5 注释：F4

对代码进行注释，即不启动代码。也可以选中代码进行批量注释

## 3.6 取消注释：F3

# 4 循环

## 4.1 if循环

```c
a:=2
if(a>0)
    a:=999
endif
```

**代码解读**：此时a被赋值为2，if进行判断`a是否大于0`,判断为True，则执行a被赋值为999，结束if循环（`end if`）

```c
a:=2
if(a>8)
    a:=999
endif
```

**代码解读**：此时a被赋值为2，if进行判断`a是否大于8`,判断为false，则不执行a被赋值为999，跳出if循环并结束if循环（`end if`）

## 4.2 for 循环

```c
g := 1
for Index := 1 to 5 by 1
    g:=g+1
endfor
```

每隔1从1运行到5（一共运行5次），每运行一次对变量g进行+1，

```c
g:=1
for index:=1 to 5 by 1
    g:=g+1
endfor
```

每隔2从1运行到5（一共运行3次），每运行一次对变量g进行+1





