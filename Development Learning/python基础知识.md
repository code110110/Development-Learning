# python基础知识

## 海象运算符 := 

原文链接：https://blog.csdn.net/weixin_44799217/article/details/129981182

海象运算符（walrus operator）是 Python 3.8 中引入的一种新的语法，其使用方法如下：

```python
variable := expression
```

其中，expression 是一个任意的表达式，而 variable 则是一个变量名。该运算符允许将表达式的结果赋值给变量，并且在同一行中进行这两个操作。

​    在某些情况下，使用海象运算符可以使代码更加简洁、易读和高效。例如，当你需要反复计算一个值并检查它是否满足某种条件时，可以使用海象运算符来减少重复代码。以下是一个示例：

```python
while (input_str := input('请输入：')) != 'exit':
    print(f"您输入的是{input_str}")
```

  在上述代码中，我们使用海象运算符来将用户输入的内容赋值给 input_str 变量，并在同一行中检查是否等于 'exit'。如果等于，则退出 while 循环；否则，打印用户输入的内容。

## 通过cv2.distanceTransform()函数将距离转换成热力图

> 此转换一般可用于视觉、SLAM中的避障部分，将可以行使的区域二值化，然后再通过distanceTransform()函数实现对目标区域从距离中心到外部的梯度，再将其转换成可视化的灰度图和热力图即可。

### 原理解析

#### （1）目标图像二值化

我们首先通过HSV阈值调节器提取出目标物体的HSV阈值，然后通过cv2.inRange函数提取出机器人可通过区域的阈值范围，得到二维平面的二值化图：

```python
inRange_hsv = cv2.inRange(hsv, color_dist['gray']['Lower'], color_dist['gray']['Upper'])
```

#### （2）高斯滤波去除噪点

可以看出图像中阈值调节后依然会有很多噪声点，所以滤波处理一下：

```python
gaussian_hsv = cv2.GaussianBlur(inRange_hsv, (3, 3), 1)  
```



#### （3）将二值化图转换成距离灰度图

距离变换函数cv2.distanceTransform()的计算结果反映了各个像素与背景（值为 0 的像素点）的距离关系。通常情况下：

```python
dist = cv2.distanceTransform(src=gaussian_hsv, distanceType=cv2.DIST_L2, maskSize=5)
```


在经过处理后，需要用**cv2.convertScaleAbs()**函数将其转回原来的**uint8**形式，否则将无法显示图像，而只是一副灰色的窗口。

```
dist1 = cv2.convertScaleAbs(dist)
```

- **dist = cv2.convertScaleAbs(src[, dst[, alpha[, beta]]])**
  - 其中可选参数alpha是伸缩系数，
  -  beta是加到结果上的一个值，
  - 结果dist返回uint8类型的图片


再将uint8类型的图片归一化处理：

```
dist2 = cv2.normalize(dist, None, 255,0, cv2.NORM_MINMAX, cv2.CV_8UC1)
```

#### （4）将距离灰度图转换成距离热力图

通过**cv2.applyColorMap()**函数，我们可以将灰度图转换成红色-蓝色的热力图，当值为255时，为红色，当值为0时，为蓝色。

```
heat_img = cv2.applyColorMap(dist2, cv2.COLORMAP_JET)
```


然后取一下RoI区域的最大值以及其坐标：

```python
costmap_roi = dist2[119:210,:]    #划定ROI区域，需要实际选取大小 现在表示选取 119-210行的位置 需要自己调整
min_value,max_value,minloc,maxloc = cv2.minMaxLoc(costmap_roi) #找最大的点
print(min_value,max_value,minloc,maxloc)
```

输出：

```
0.0 255.0 (151, 0) (0, 28)
```

我们将值最大的点绘制在图中看一下效果：

```
cv2.circle(heat_img2, (maxloc), 1, (0,255,0), -1)
```

可以看出它很好的找到了RoI区域距离障碍物最远的点。

代码实现

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

color_dist = {'gray': {'Lower': np.array([0,0,46]), 'Upper': np.array([180,43,220])},
			  'red': {'Lower': np.array([0, 0, 0]), 'Upper': np.array([255, 93, 255])},
              'blue': {'Lower': np.array([100, 80, 46]), 'Upper': np.array([124, 255, 255])},
              'yellow': {'Lower': np.array([26, 43, 46]), 'Upper': np.array([34, 255, 255])},
              }

# img = cv2.imread("test_pictures/red_car.jpg")

img = cv2.imread("test_pictures/62.jpg")
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
inRange_hsv = cv2.inRange(hsv, color_dist['gray']['Lower'], color_dist['gray']['Upper'])

gaussian_hsv = cv2.GaussianBlur(inRange_hsv, (3, 3), 1)  # 高斯滤波，去除小的噪声

dist = cv2.distanceTransform(src=gaussian_hsv, distanceType=cv2.DIST_L2, maskSize=5)
dist1 = cv2.convertScaleAbs(dist)
dist2 = cv2.normalize(dist, None, 255,0, cv2.NORM_MINMAX, cv2.CV_8UC1)
heat_img = cv2.applyColorMap(dist2, cv2.COLORMAP_JET)
costmap_roi = dist2[119:210,:]    #划定ROI区域，需要实际选取大小 现在表示选取 0-179行的位置 需要自己调整
heat_img2 = cv2.applyColorMap(costmap_roi, cv2.COLORMAP_JET)
min_value,max_value,minloc,maxloc = cv2.minMaxLoc(costmap_roi) #找最大的点
print(min_value,max_value,minloc,maxloc)
cv2.circle(heat_img2, (maxloc), 2, (0,255,0), -1)

cv2.imshow("img", img)
cv2.imshow("dist", dist)
cv2.imshow("dist1", dist1)
cv2.imshow("dist2", dist2)
cv2.imshow("heat_img", heat_img)

cv2.waitKey()
```