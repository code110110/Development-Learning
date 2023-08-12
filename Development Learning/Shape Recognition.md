# Shape Recognition

[关于形状识别的论文阅读笔记](https://richarddzh.gitbooks.io/book1/content/part_i_shape_recognition.html)

## 1 Chamfer Matching

Chamfer matching是一种进行图像匹配的方法，最早见于(Barrow,1977)

### 构造距离变换

方法的名字chamfer指的是一个求取距离变换(DT, distance transform, distance function)的过程。 距离变化的一个例子如下。对于一个有特征点(*)和非特征点(-)组成的二值图像，距离变换就是求得每一个点到最近特征点的距离。早期文献中用到的特征点都是边缘点。

![Distance transform](https://richarddzh.gitbooks.io/book1/content/images/distance-transform.png)

上图来自(Borgefors,1986)这篇文章，文章详细描述了计算DT的方法。计算是一个迭代的过程，计算的过程可以理解为类似Floyd–Warshall算法的**[任意点对最短路算法](https://blog.csdn.net/damontive/article/details/79115842)**。计算的距离为整数值，并不是简单的计算欧氏距离。这些值是由一个mask来定义的。

![img](https://richarddzh.gitbooks.io/book1/content/images/distance-transform-mask.png)

有了这样一个mask，在第m次迭代时，点(i,j)处的距离v就可以按照如下的方式计算：
$$
v^{(m)}_{i,j}=min_{(k,l)\in mask}(v^{(m−1)}_{i+k,j+l}+c(k,l))
$$
这个迭代式可以按照并行或者串行的方式迭代计算。按照并行方式的话，就是每次在每个位置上应用整个mask。按照串行方式，则是将这个mask按照加粗的线条分为上下两个部分，上半部分为forward mask，下半部分为backward mask。当从图像左上角向右下计算时，应用forward mask；从右下角向左上计算时，应用backward mask；如此交替进行。

显然，mask的形式影响到计算的结果。文章(Borgefors,1988)认为，欧氏距离不是最好的选择（可能受限于当时的计算能力，作者认为图像本身并不精确，不需要计算欧氏距离）。作者给出了一个3-4DT的方法，mask的尺寸为3x3，近似模拟了欧式距离的比例(1:1.414约为3:4)。

### 匹配过程

对两幅图像进行匹配的过程是这样的：为其中一幅计算DT，然后将另一幅的特征点叠加在DT上，计算特征点对应的DT值的均值。这是原始的论文给出的方法。下图显示了将待匹配的曲线叠加在DT上。曲线和图像的距离就可以通过叠加点上DT值的某种均值来计算。

![img](https://richarddzh.gitbooks.io/book1/content/images/polygon-over-dt.png)

后来的文章(Borgefors,1988)认为，采用均值不是最佳的策略，为了避免假的匹配，一种更好地方法是计算root mean square (r.m.s)：
$$
\frac{1}{3}\sqrt{\frac{1}{2}\sum_{i=1}^n v_i ^2}
$$
匹配的图像可以在DT上平移、旋转、缩放，因此，匹配的过程就是寻找到一个最优的位置。由于存在众多的局部极小点，这个优化过程是不能够简单的采用某些梯度下降的方法得到的。

### Hierarchical Chamfer Matching

文章(Borgefors,1988)给出了一种优化的过程。 从文章的题目可知，第一步是构建图像金字塔， 构建的方法是对原始边缘图进行比例为2的降采样。 降采样的方法是在2x2的小块上进行OR，即或运算。 这一步得到的叫做边缘金字塔。对边缘金字塔进行距离变换，得到的叫做距离金字塔。

用来构建距离金字塔的图像叫做pre-distance image。带匹配的图像叫做pre-polygon image。 在将polygon叠加在DT上之前，可以进行一些变换，这些变换用参数表示，比如平移可以用一对参数(x,y)表示。优化的过程就是按照一定的步长(dx,dy)调整参数，在参数空间的邻近点上寻找局部最优值。比如，调整x，就可以比较f(x+dx),f(x),f(x-dx)获取x的优化方向，如此迭代直至找到局部最优。当参数维度较大时，同时比较参数空间各个维度上的邻近点会导致严重的性能问题。因此，文章采取住个参数调整的方法。

优化的过程从一个较粗的尺度开始，以若干个不同的参数值为起点，分别独立的进行优化。在得到一组局部最优点后，根据一些原则拒绝掉其中的一部分。剩余的局部最优点再次作为起点，在下一个尺度层次上进行优化。

### 参考文献

1. >Barrow, H. G., Tenenbaum, J. M., Bolles, R. C., & Wolf, H. C. (1977). Parametric correspondence and chamfer matching: Two new techniques for image matching.

2. > Borgefors, G. (1986). Distance transformations in digital images. Computer vision, graphics, and image processing, 34(3), 344-371.

3. > Borgefors, G. (1988). Hierarchical chamfer matching: A parametric edge matching algorithm. IEEE T-PAMI, 10(6), 849-865.

图像处理中的倒角距离变换(Chamfer Distance Transform)在对象匹配识别中经常用到，

算法基本上是基于3x3的窗口来生成每个像素的距离值，分为两步完成距离变换，第一

步从左上角开始，从左向右、从上到下移动窗口扫描每个像素，检测在中心像素x的周

围0、1、2、3四个像素，保存最小距离与位置作为结果，图示如下：

![img](http://img.blog.csdn.net/20150111230223203)

第二步从底向上、从右向左，对每个像素，检测相邻像素4、5、6、7保存最小距离与

位置作为结果，如图示所：

![img](http://img.blog.csdn.net/20150111230248887)

完成这两步以后，得到的结果输出即为倒角距离变换的结果。完整的图像倒角距离变

换代码实现可以分为如下几步：

1.   对像素数组进行初始化，所有背景颜色像素点初始距离为无穷大，前景像素点距离为0
2.   开始倒角距离变换中的第一步，并保存结果
3.   基于第一步结果完成倒角距离变换中的第二步
4.   根据距离变换结果显示所有不同灰度值，形成图像

最终结果显示如下(左边表示原图、右边表示CDT之后结果)

完整的二值图像倒角距离变换的源代码如下：

```python
package com.gloomyfish.image.transform;

import java.awt.Color;
import java.awt.image.BufferedImage;
import java.util.Arrays;

import com.gloomyfish.filter.study.AbstractBufferedImageOp;

public class CDTFilter extends AbstractBufferedImageOp {
	private float[] dis; // nn-distances
	private int[] pos; // nn-positions, 32 bit index
	private Color bakcgroundColor;
	
	public CDTFilter(Color bgColor)
	{
		this.bakcgroundColor = bgColor;
	}

	@Override
	public BufferedImage filter(BufferedImage src, BufferedImage dest) {
		int width = src.getWidth();
		int height = src.getHeight();

		if (dest == null)
			dest = createCompatibleDestImage(src, null);

		int[] inPixels = new int[width * height];
		pos = new int[width * height];
		dis = new float[width * height];
		src.getRGB(0, 0, width, height, inPixels, 0, width);
		// 随机生成距离变换点
		int index = 0;
		Arrays.fill(dis, Float.MAX_VALUE);
		int numOfFC = 0;
		for (int row = 0; row < height; row++) {
			for (int col = 0; col < width; col++) {
				index = row * width + col;
				if (inPixels[index] != bakcgroundColor.getRGB()) {
					dis[index] = 0;
					pos[index] = index;
					numOfFC++;
				}
			}
		}
		final float d1 = 1;
		final float d2 = (float) Math.sqrt(d1 * d1 + d1 * d1);
		System.out.println(numOfFC);
		float nd, nd_tmp;
		int i, in, cols, rows, nearestPixel;

		// 1 2 3
		// 0 i 4
		// 7 6 5
		// first pass: forward -> L->R, T-B
		for (rows = 1; rows < height - 1; rows++) {
			for (cols = 1; cols < width - 1; cols++) {
				i = rows * width + cols;

				nd = dis[i];
				nearestPixel = pos[i];
				if (nd != 0) { // skip background pixels
					in = i;

					in += -1; // 0
					if ((nd_tmp = d1 + dis[in]) < nd) {
						nd = nd_tmp;
						nearestPixel = pos[in];
					}

					in += -width; // 1
					if ((nd_tmp = d2 + dis[in]) < nd) {
						nd = nd_tmp;
						nearestPixel = pos[in];
					}

					in += +1; // 2
					if ((nd_tmp = d1 + dis[in]) < nd) {
						nd = nd_tmp;
						nearestPixel = pos[in];
					}

					in += +1; // 3
					if ((nd_tmp = d2 + dis[in]) < nd) {
						nd = nd_tmp;
						nearestPixel = pos[in];
					}

					dis[i] = nd;
					pos[i] = nearestPixel;
				}
			}
		}

		// second pass: backwards -> R->L, B-T
		// exactly same as first pass, just in the reverse direction
		for (rows = height - 2; rows >= 1; rows--) {
			for (cols = width - 2; cols >= 1; cols--) {
				i = rows * width + cols;

				nd = dis[i];
				nearestPixel = pos[i];
				if (nd != 0) {
					in = i;

					in += +1; // 4
					if ((nd_tmp = d1 + dis[in]) < nd) {
						nd = nd_tmp;
						nearestPixel = pos[in];
					}

					in += +width; // 5
					if ((nd_tmp = d2 + dis[in]) < nd) {
						nd = nd_tmp;
						nearestPixel = pos[in];
					}

					in += -1; // 6
					if ((nd_tmp = d1 + dis[in]) < nd) {
						nd = nd_tmp;
						nearestPixel = pos[in];
					}

					in += -1; // 7
					if ((nd_tmp = d2 + dis[in]) < nd) {
						nd = nd_tmp;
						nearestPixel = pos[in];
					}

					dis[i] = nd;
					pos[i] = nearestPixel;

				}
			}
		}

		for (int row = 0; row < height; row++) {
			for (int col = 0; col < width; col++) {
				index = row * width + col;
				if (Float.MAX_VALUE != dis[index]) {
					int gray = clamp((int) (dis[index]));
					inPixels[index] = (255 << 24) | (gray << 16) | (gray << 8)
							| gray;
				}
			}
		}
		setRGB(dest, 0, 0, width, height, inPixels);
		return dest;
	}

	private int clamp(int i) {
		return i > 255 ? 255 : (i < 0 ? 0 : i);
	}

}
```

## 1.1 快速定向倒角匹配*

以单个手绘示例或形状库作为对象模型来研究图像中的对象定位问题。显着提高了倒角匹配的精度，同时将计算时间从线性减少到次线性（根据经验显示）。具体来说，将边缘方向信息合并到匹配算法中，使得生成的成本函数是分段平滑的并且成本变化是紧密有界的。此外，我们提出了一种次线性时间算法，使用 **3D 距离变换**和**方向积分图像技术来**精确计算方向倒角匹配分数。此外，平滑的成本函数允许限制大邻域的成本分布并跳过其中的不良假设。实验表明，所提出的方法将原始倒角匹配的速度提高了 45 倍，并且比许多现有技术快得多，同时精度相当。

提出了一种合并边缘方向并解决方向增强空间中的匹配问题的替代方法。该公式在定义边缘对应关系方面发挥着积极作用，从而在高度混乱的环境中实现更稳健的匹配。得到的成本函数是平滑的并且成本变化是严格限制的。即使没有方向项，现有倒角匹配算法的最佳计算复杂度与模板边缘点的数量成线性关系。我们分三个阶段优化方向匹配成本：

- (1)我们提出模板边缘的线性表示。 
-   (2)然后我们描述三维距离变换表示。
-   (3)最后，我们提出了距离变换的方向积分图像表示。

使用这些中间结构，可以在边缘点数量的亚线性时间内执行匹配分数的精确计算。在存在许多形状模板的情况下，内存需求也大大减少。此外，平滑的成本函数允许限制大型邻域的成本分布，并跳过其中的不良假设。

### 2 倒角匹配 

倒角匹配 (CM) [1] 是一种流行的技术，用于查找两个边缘图之间的最佳对齐方式。令 $U = \{ u_i\}$$ 和 V = \{v_j\}$​ 分别为模板和查询图像边缘图的集合。 U 和 V 之间的倒角距离由每个点 $u_i ∈ U $​与其在 V 中最近的边缘之间的距离平均值给出。
$$
d_{CM}(U,V)=\frac{1}{n}\sum_{u_i \in U} \mathbf{min}_{v_j \in V}|u_i-v_j| \\ n = |U|.
\tag{1}
$$

令 W 为在用 s 参数化的图像平面上定义的扭曲函数。例如，如果是 2D 欧几里得变换，则 $s ∈ SE(2), s = (θ, t_x, t_y)$，其中$t_x$ 和$t_y $​分别是沿 x 轴和 y 轴的平移，θ 是面内旋转角度。它对图像点的作用是通过变换给出的
$$
\mathbf{W}(\mathbf{x} ; \mathbf{s})=\left(\begin{array}{cc}
\cos (\theta) & -\sin (\theta) \\
\sin (\theta) & \cos (\theta)
\end{array}\right) \mathbf{x}+\left(\begin{array}{c}
t_{x} \\
t_{y}
\end{array}\right)
\tag{2}
$$
两个边缘图之间的最佳对齐参数 $\hat{s} ∈ SE(2) $由下式给出
$$
\hat{s}=arg_{s \in SE(2)}d_{CM}(\mathbf{W}(U;s),v)
\tag{3}
$$
其中$\mathbf{W}(U;s)=\{\mathbf{W}(u_i,s)\}$倒角匹配提供了相当平滑的适应性测量，并且可以容忍小的旋转、未对准、遮挡和变形。匹配成本可以通过距离变换图像 $DT_V (\mathbf{x}) = \mathbf{min_{v_j∈V} |x − v_j|} $有效计算它指定 V 中每个像素到最近边缘像素的距离。距离变换可以在图像 [6] 上分两遍进行计算，并且可以通过$ d_{CM}(U, V ) = \frac{1}{ n} \sum_{u_i\in U}DT_V(u_i) $在线性时间 O(n) 内评估成本函数 (1)。

### 3 定向倒角匹配

在存在背景杂乱的情况下，倒角匹配变得不太可靠。为了提高鲁棒性，通过将边缘方向信息合并到匹配成本中，引入了倒角匹配的几种变体。在[5, 11]中，模板和查询图像边缘被量化为离散方向通道，并对通道间的各个匹配分数进行求和。虽然这种方法缓解了场景混乱的问题，但成本函数对方向通道的数量非常敏感，并且在通道边界上变得不连续。在[16]中，倒角距离随着方向不匹配的额外成本而增加，该成本由模板边缘与其在查询图像中最近的边缘点之间的方向的平均差异给出。该方法称为定向倒角匹配，在整篇论文中我们使用缩写 OCM(Oriented Chamfer Matching)。

我们没有明确地表述方向适配，而是将倒角距离推广到 $\mathbb{R}^3$ 中的点以匹配方向边缘像素。每个边缘点 x 都用方向项 $\phi(\mathbf{x})$ 进行增强，方向倒角匹配 (DCM) 分数由下式给出.
$$
d_{DCM}(U,V)=\frac{1}{n}\sum_{u_i\in U}min_{v_j\in V}|u_i-v_j|+	\lambda|\phi(u_i)-\phi(v_j)|
\tag{4}
$$
其中 λ 是位置和方向项之间的权重因子。请注意，方向 $\phi(x)$以模 $\pi$ 计算，方向误差给出两个方向之间的最小圆差 min$\{|\phi(\mathbf{x_1}) − \phi \mathbf{(x_2)}|, ||\phi(\mathbf{x_1}) − \phi \mathbf{(x_2)}| − π|\}$。

![image-20230807132335291](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\image-20230807132335291.png)

> **图 1. (a) 定向倒角匹配 [16] 的每个边缘点的匹配成本； (b) 定向倒角匹配。 DCM 共同最小化位置和方向误差，而在[16]中，位置误差随着最近边缘点的方向误差而增加。**

![image-20230807132435554](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\image-20230807132435554.png)

> **图 2. 线性表示。 (a) 边缘图像。该图像包含 1242 个边缘点。 (b) 边缘图像的线性表示。该图像包含 60 条线段。**

在图 1 中，我们将所提出的成本函数与 [16] 进行了比较。可以很容易地验证所提出的匹配成本是模板边的平移 tx、ty 和旋转 θ 的分段平滑函数。因此，匹配对于杂乱、丢失边缘和小偏差来说更加稳健。

### 4. 搜索优化

据我们所知，现有倒角匹配算法的最低计算复杂度与模板边缘点的数量成线性关系，即使没有方向项。在本节中，我们提出了一种亚线性时间算法，用于精确计算 3D 倒角匹配分数 (4)

#### 4.1. 线性表示

场景的边缘图不遵循非结构化的二进制模式。相反，对象轮廓符合某些连续性约束，可以通过连接不同长度、方向和平移的线段来保留这些连续性约束。在这里，我们用 m 线段的集合表示边缘图像。与基数为n的点集相比，其线性表示更加简洁。它只需要 $O(m) $内存来存储 $m < < n$ 的边缘图。我们使用 RANSAC 算法的变体来计算边缘图的线性表示。该算法的概要如下。该算法最初通过选择一小部分点及其方向来假设各种直线。直线的支撑由在小残差内满足直线方程并形成连续结构的点集给出。保留具有最大支持的线段，并使用缩减集迭代该过程，直到支持变得小于几个点。

该算法只保留具有一定结构和支持度的点，从而滤除噪声。此外，与使用局部算子（例如图像梯度）计算的方向相比，通过线拟合过程恢复的方向更加精确。图 2 给出了线性表示的示例，其中使用 60 条线段对一组 1242 个点进行建模。

![image-20230807133351175](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\image-20230807133351175.png)

>  **图 3 积分距离变换张量的计算。(a) 输入边缘图。 (b) 边缘被量化为离散方向通道。 (c) 每个方向通道的二维距离变换。 (d)基于定向成本更新三维距离变换$DT3$ 。 (e) 沿离散边缘方向对 $DT3$ 张量进行积分，并计算积分距离变换张量 $IDT3$。**

#### 4.2.三维距离变换

(4) 中给出的匹配分数需要找到每个模板边缘点的位置和方向项的最小成本匹配。因此，暴力算法的计算复杂度是模板和查询图像边缘点数量的二次方。在这里，我们提出了一个三维距离变换表示$（DT3）$来计算线性时间内的匹配成本。我们注意到，[14]中也使用了类似的结构来快速评估豪斯多夫距离。

该表示是三维图像张量，其中前两个维度是图像平面上的位置，第三个维度是量化的边缘方向。方向在$ [0 , π) $范围内均匀地量化为$q$个离散通道 $\hatΦ = \{ \hat{\phi}_i\}$。张量的每个元素都编码到关节位置和方向空间中的边缘点的最小距离：
$$
D T 3_{V}(\mathbf{x}, \phi(\mathbf{x}))=\min _{\mathbf{v}_{i} \in V}\left|\mathbf{x}-\mathbf{v}_{j}\right|+\lambda\left|\hat{\phi}(\mathbf{x})-\hat{\phi}\left(\mathbf{v}_{j}\right)\right| .
\tag{5}
$$
其中 $\hat\phi(x)$ 是方向空间中最接近 $\hat{\Phi}$ 中 $\phi(x)$ 的量化级别.

我们提出了一种算法，通过连续求解两个动态程序，在$ O(q) $时间内计算 $DT3$ 张量。方程(5)可以改写为
$$
D T 3_{V}(\mathbf{x}, \phi(\mathbf{x}))=\min _{\hat{\phi}_{i} \in \hat{\Phi}}\left(D T_{V\left\{\hat{\phi}_{i}\right\}}+\lambda\left|\hat{\phi}(\mathbf{x})-\phi_{i}\right|\right).
\tag{6}
$$
其中 $D T_{V\left\{\hat{\phi}_{i}\right\}}$ 是 V 中具有方向$\hat{\phi_i}$i 的边缘点的二维距离变换。

最初，我们计算 q 个二维距离变换$ DT_V \{ \hat\phi_i\}$，这需要使用标准距离变换算法 [6] 对图像进行 $O(q) $次传递。随后，通过分别求解每个位置的第二个动态程序来计算 $DT3_V$ 张量 (6)。张量使用二维距离变换 $DT_V \{\mathbf{x} ,\hat\phi_i\}= DT_{V{} \{ \hat\phi_i\}}(\mathbf x)$ 进行初始化，并使用前向递归进行更新.
$$
\begin{aligned}DT3_V(\mathbf{x},\hat{\phi}_i)=min\quad&\{DT3_V(\mathbf{x},\hat{\phi}_i),DT3_V(\mathbf{x},\hat{\phi}_{i-1})+\lambda|\hat{\phi}_{i-1}-\hat{\phi}_i|\}\end{aligned}
\tag{7}
$$
和向后递归:
$$
\begin{aligned}DT3_V(\mathbf{x},\hat{\phi}_i)=min\quad&\{DT3_V(\mathbf{x},\hat{\phi}_i),DT3_V(\mathbf{x},\hat{\phi}_{i+1})+\lambda|\hat{\phi}_{i+1}-\hat{\phi}_i|\}\end{aligned}
\tag{8}
$$
对于每个位置$ \mathbf{x}$。与标准距离变换算法不同，处理圆形方向距离需要特殊条件。前向和后向递归不会在分别为$ i = 1 \cdots q$ 或 $i = q \cdots1$的完整循环后终止，但成本继续以循环形式更新，直到张量条目的成本不改变。请注意，每个前向和后向递归最多需要一个半周期，因此最差的计算成本是$ O(q)$ 遍历图像。使用三维距离变换表示 $DT 3_V$，任何模板 U 的方向倒角匹配分数可以通过以下方式在线性时间内计算：
$$
d_{DCM}(U,V)=\dfrac{1}{n}\sum_{\mathbf{u}_i\in U}DT3_V(\mathbf{u}_i,\hat{\phi}(\mathbf{u}_i)).
\tag{9}
$$

#### 4.3. 距离变换积分

令 $L_{U}=\{l_{[\mathbf{s}_{i},\mathbf{e}_{i}]}\}_{j=1\dots m} $为模板边缘点 U 的线性表示，其中$ s_j $和 $e_j $分别是第 j 条线的起始位置和结束位置。为了便于表示，我们有时只使用索引$ l_j = l[s_j,e_j ] $来引用一行。我们假设线段仅在 $q$ 个离散通道$ \hat\phi $之间具有方向，这是在计算线性表示时强制执行的。尽管可能有人认为离散线方向引入了量化伪影，但实际上图 2b 中给出的线性表示仅使用$ q = 6 0$ 方向生成，并且很难观察到与原始边缘图像（图 2a）的差异。

线段上的所有点都与同一方向相关联，即直线 $\hat\phi(l_j)$ 的方向。因此，方向倒角匹配分数 (9) 可以重新排列为

$$
d_{DCM}(U,V)=\frac{1}{n}\sum_{l_j\in L_U} \sum _{\mathbf{u}_i\in l_j}DT3_V(\mathbf{u}_i,\hat{\phi}(l_j)).
\tag{10}
$$
在此公式中，仅评估 $DT3_V $张量$ DT3_V (\mathbf{x}, \hat\phi_i)$ 的第 i 个方向通道，以对具有方向 $\hat\phi_i$的线段点求和。

积分图像是用于快速计算区域和[19]和线性和[2]的中间图像表示。在这里，我们提出了积分距离变换表示的张量 $(IDT3_V )$，以评估 $O(1) $操作中任何线段上的成本总和。对于每个方向通道$ i$，我们计算沿 $\hat\phi_i$ 的单向积分（图 3）。

设$ x_0$ 为图像边界与穿过 x 且方向为$\hat\phi_i$的线的交点。 $IDT3_V $张量的每个条目由下式给出
$$
IDT3_V(\mathbf{x},\hat{\phi_i})=\sum_{\mathbf{x}_j\in l_{[\mathbf{x}_0,\mathbf{x}]}}DT3_V(\mathbf{x}_j,\hat{\phi}_i).
\tag{11}
$$
$IDT3_V $张量可以通过 $DT3_V $张量一次性计算出来。使用这种表示，任何模板$ U $的方向倒角匹配分数都可以通过以下 $O(m) $操作计算出来：
$$
\begin{aligned}d_{DCM}(U,V) = \frac{1}{n}\sum_{l_{[\mathbf{s}_{j},\mathbf{e}_{j}]}\in L_{U}}[IDT3_{V}(\mathbf{e}_{j},\hat{\phi}(l_{[\mathbf{s}_{j},\mathbf{e}_{j}]}))- IDT3_{V}(\mathbf{s}_{j},\hat{\phi}(l_{[\mathbf{s}_{j},\mathbf{e}_{j}]}))].\end{aligned}
\tag{12}
$$
$O(m) $复杂度只是计算次数的上限。对于对象检测或定位，我们希望仅保留匹配成本小于检测阈值的假设，或者等效地对于定位小于最佳假设。我们根据模板线的支持度对模板线进行排序，并从具有最大支持度的线开始求和。如果成本大于检测阈值或当前最佳假设，则在求和过程中消除假设。线段的支持度呈现指数衰减，因此对于大多数假设，仅执行少量算术运算。我们凭经验表明，评估的线段数量与模板点$ n $的数量呈次线性关系。

#### 4.4. 优化区域搜索

他提出的DCM成本函数(4)是平滑的。此外，匹配成本的变化是有界的，并且仅在空间域中平滑变化。我们利用这一事实来显着减少从图像评估的假设数量。令 $δ ∈ \mathbb{R}^2$ 为模型 $U$ 在图像平面中的平移。由于翻译而导致的 $DCM$ 成本变化变为:
$$
\begin{aligned}
&d_{DCM}(U+\delta,V) \\
&={\frac{1}{n}}\sum_{\mathbf{u}_{i}\in U}\operatorname*{min}_{\mathbf{v}_{j}\in V}|\mathbf{u}_{i}+\delta-\mathbf{v}_{j}|+\lambda|\phi(\mathbf{u}_{i})-\phi(\mathbf{v}_{j})| \\
&\leq{\frac{1}{n}}\sum_{\mathbf{u}_{i}\in U}\operatorname*{min}_{\mathbf{v}_{j}\in V}|\mathbf{u}_{i}-\mathbf{v}_{j}|+|\delta|+\lambda|\phi(\mathbf{u}_{i})-\phi(\mathbf{v}_{j})| \\
&=|\delta|+d_{DCM}(U,V).
\end{aligned}
\tag{13}
$$
因此，DCM 成本的变化受到空间平移 $|d_{DCM} (U +δ, V )−d_{DCM} (U, V )| ≤|δ| $的限制。。如果目标匹配成本是 $\epsilon$当前假设的成本是$\psi,\mathbf{i.e.} \epsilon <\psi$在 $|\psi-\epsilon|$ 范围内不能进行检测当前假设的像素范围，我们可以跳过对该区域内假设的评估。

### 5. 实验

我们对具有挑战性的合成数据集和真实数据集进行了三组实验。请注意，在我们所有的实验中，我们强调与倒角匹配相比，我们的方法的速度和准确性的提高。尽管所提出的方法的性能与最先进的方法相当，但它也可以用于快速检索准确的初始假设，然后可以使用更昂贵的点配准算法对其进行细化。在我们所有的实验中，我们使用 $q = 6 0$ 个方向通道，6 度方向误差对应于 1 像素距离。

#### 5.1. 物体检测和定位

在第一个实验中，我们在 ETHZ 形状类数据集 [10] 上进行了对象检测和定位。该数据集包含 255 张图像，其中每张图像包含来自五个不同类别的一个或多个对象：苹果徽标、瓶子、长颈鹿、杯子和天鹅。数据集中的对象在外观、视点、大小和非刚性变形方面有很大的变化。我们遵循[10, 9]中提出的实验设置，其中每个类使用单个手绘形状来检测和定位数据集中的实例。

我们的检测系统基于使用滑动窗口的扫描，即，我们保留成本函数小于检测阈值的所有假设。为了说明算法的速度，我们对假设空间进行密集采样，并以 8 个不同尺度和 3 个不同长宽比搜索图像。两个连续比例之间的比率为1.2，纵横比为1.1。我们通过仅保留具有显着重叠的假设中的最低成本假设来执行非极大值抑制。

在图 4 中，我们报告了每张图像的误报与检测率。该曲线是通过改变匹配成本的检测阈值来生成的。我们将我们的方法与定向倒角匹配 [16] 以及 Ferrari 等人最近提出的两项研究进行了比较。等人。 [10, 9]。我们的方法在所有误报率上都明显优于定向倒角匹配，并且与[9]相当，其中我们的结果对于两个类别（长颈鹿和瓶子）更好，而对于天鹅类别稍差，而对于其他两个类别，数字几乎相同。如检测示例（图 4）所示，对象定位非常准确。我们注意到，在[20]和[15]中，该数据集的性能稍好一些。由于这些结果以不同的格式呈现，我们无法将它们包含在我们的图表中。

在表 1 中，我们列出了每个假设的匹配成本的平均评估时间。形状模板中的平均点数为 1610，根据五个类别计算得出。同样，我们的线性表示平均每类包含 39 行。每个类的行数只是计算次数的上限。由于该算法仅检索成本小于检测阈值的假设，因此当成本达到该值时，终止对假设的求和。每个假设平均仅评估 14 个品系。结果表明，该方法将倒角匹配速度提高了 43 倍，将定向倒角匹配速度提高了 127 倍。请注意，对于较大尺寸的模板，速度提升更为显着，因为我们的成本计算对模板尺寸不敏感，而原始倒角匹配的成本线性增加。

平均而言，我们评估每张图像 105 万个假设，耗时 0.42 秒。使用第 4.4 节中介绍的边界技术，我们进一步将每个图像的平均处理时间减少到 0.39 秒，其中大约 91% 的假设被跳过。请注意，在使用边界函数时，我们需要计算每个评估假设的完整成本函数（所有行的总和）。因此，加速不成正比。

#### 5.2. 人体姿态估计

在第二个实验中，我们利用派生的形状匹配框架进行人体姿势估计，由于人体姿势的外观变化很大，这是一项非常具有挑战性的任务。正如[13]中所提出的，我们将具有已知姿势的人体形状库与给定的观察相匹配。由于关节的原因，精确姿势估计所需的姿势库的大小相当大。因此，拥有一种能够应对背景杂波的有效匹配算法变得越来越重要。

实验还在 HumanEva 数据集 [17] 上进行，该数据集包含从不同观看方向捕获的多个人类受试者执行各种活动的视频序列。使用附加的标记提取每个图像中人体关节的地面真实位置。形状库模板分两步获取。首先，我们通过 HumanEva 背景减法代码计算人体轮廓。然后，使用提取的轮廓轮廓周围的 Canny 边缘，我们获得了形状模板。我们将动作序列（大约 1,000 - 2,000 个）中的所有图像包含到形状库中，然后使用形状库来估计不同序列中的人体姿势。当我们直接从测试图像中提取 Canny 边缘时，它们包含大量的背景杂波。然后通过匹配框架检索最佳形状模板及其比例和位置。我们定量评估了地面实况标记位置与图像平面上估计姿态之间的平均绝对误差。结果如表 2 所示，我们观察到与倒角匹配和定向倒角匹配相比，精度显着提高。所提出的方法每秒可以评估超过 110 万个假设，而倒角匹配和定向倒角匹配每秒可以分别评估 31000 和 14000 个假设。图 5 给出了几个姿态估计示例。

# 2 Partial Contour Matching

## 基于轮廓点夹角的方法

Riemenschneider(2010)提出了一种基于形状的物体识别方法，该方法将图像中的曲线段和模板轮廓线进行匹配，就用到了部分匹配的方法。

## 轮廓片段的描述

文章采用的曲线描述方式来自作者更早的工作(Donoser,2009)，该工作用于描述和比较完整闭合轮廓。 而(Riemenschneider,2010)则将描述稍作改进，用于描述非闭合曲线。

一个形状的轮廓曲线（或者图像中的非闭合曲线）首先要等距采样为一个点序列 . 然后用一个矩阵描述每三个点之间的夹角。 这是一个的矩阵，其中的元素定义如下。 下图就是一个示意图，显示了定义角度的两种不同情况。

![img](https://richarddzh.gitbooks.io/book1/content/images/angle-matrix.png)

角度组成的矩阵就可以描述轮廓片段的特征。 下面是一些基本的轮廓元素对应的矩阵。 其中，第一行是(Donoser,2009)的矩阵； 第二行是(Riemenschneider,2010)改进后的矩阵。 根据矩阵元素的定义，这种描述轮廓片段的方式天然的具有平移和旋转不变性，但是没有尺度不变形。 后续的匹配和识别算法要在多个尺度上进行。 该矩阵的另一个特点是，通过沿着对角线进行移位，可以解决闭合轮廓在采样为点序列时起始位置选择的问题。

![img](https://richarddzh.gitbooks.io/book1/content/images/angle-matrix-demo.png)

## 匹配算法

首先定义匹配。两条曲线的某一部分相匹配，就是对应的角度矩阵中沿着对角线的两个子矩阵差异比较小。下图比较了一把合拢的剪刀和一把张开的剪刀的轮廓曲线，可以明显看到四组对应的子矩阵。

![img](https://richarddzh.gitbooks.io/book1/content/images/angle-matrix-match.png)

形式化的定义如下，和是两条曲线对应的描述矩阵，下面定义了从上的r点开始的长度l的片段和从上的q点开始的长度为l的片段的差异度。 我们设两条曲线长度分别为M和N，不妨设M较小，那么，所有的不同的片段的差异度就构成了一个三维的张量(tensor)，或者说N个的矩阵。因此，我们可以把D视作一个张量。

快速计算张量D，需要用到一点技巧，积分图像(integral image)，用于快速计算图像中的任意矩形区域的特征（比如矩形区域像素和）。文章(Viola,2001)详细描述了该方法。一个图像i的积分图像ii中的每一个点的值是该点左上的矩形区域内像素值的和，即. 这样，任意的矩形区域的和可以由积分图像快速计算（常数时间）。 快速计算张量D的方法是，首先计算N个不同的积分图像，分别对应于将矩阵沿着其对角线移位得到的N个不同的矩阵和矩阵的差值的积分图像。有了这N个积分图像，就可以快速计算张量D了。

在张量D的基础上，进一步筛选长度和相似度符合要求的匹配。为此，定义如下的函数。 进一步将这个函数转化为2元函数，，就可以求得该函数的局部极大值，这些局部极大值就对应于一些满足长度和相似度要求的匹配。

## 识别物体：假设投票

在上面的匹配过程中，假设曲线A1是模板(reference)，曲线A2是测试图像中的曲线(query)。那么，得到的每一对匹配就表示模板有可能出现在图像之中，并且可以估计出现的位置，这就构成了一个假设，于是可以用大量匹配进行假设投票。 投票的结果就是物体识别的结果。

在(Donoser,2009)中，作者采用了另外的方法进行形状提取。 与物体识别不同，形状提取不需要考虑背景线条的影响，也不考虑线条可能会破碎。 形状提取需要考虑的主要因素是，两个形状在多大程度上相似。 对于存在差异的两个形状，对比的局部越小，差异度就越小；对比的局部越大，差异度就越大。 作者的方法遵循了这样的观点，在(Bronstein,2009)中，对部分相似性进行了形式化的定义和计算。

## 参考文献

1. > Riemenschneider, H., Donoser, M., & Bischof, H. (2010). Using partial edge contour matches for efficient object category localization. In ECCV 2010.

2. > Donoser, M., Riemenschneider, H., & Bischof, H. (2009). Efficient partial shape matching of outer contours. In ACCV 2009.

3. > Viola, P., & Jones, M. (2001). Rapid object detection using a boosted cascade of simple features. In CVPR 2001.

4. > Bronstein, A. M., Bronstein, M. M., Bruckstein, A. M., & Kimmel, R. (2009). Partial similarity of objects, or how to compare a centaur to a horse. IJCV, 84(2), 163-183.

# 3. Fan Shape Model

## 轮廓描述

这是一种基于形状的物体检测模型，见于文章(Wang,2012)。 在形状S的轮廓上致密均匀的采样一些点 ， 在形状内部取一个参考点o，然后可以将形状描述为n个向量，每个轮廓点对应一个向量。 其中，是向量的辐角，是向量的模长，则是对应轮廓点附近的边缘朝向。 文章中，这样的向量称作ray（射线）。

![img](https://richarddzh.gitbooks.io/book1/content/images/fan-model-vector.png)

在上述定义的基础上，可以计算两个ray的差异。 从而，可以定义两段轮廓的差异。 这里，和分别有n和m个采样点，是从{1...n}到{0...m}的函数，函数值取0表示没有匹配。匹配是有序的，因此函数在取非0值时是单调递增的。 匹配的目标是求得能够最小化的函数，这个过程可以用动态规划算法完成。

## Fan Shape Modle 形状模型

上述轮廓描述仍不能作为一类形状的模型。 为此，作者提出了FSM模型。 上面将某一轮廓描述为一系列rays， 那么，对于物体类，每一个ray就不再是确定的一个向量，而是描述该向量可能取值的一个概率分布。 将分布用参数表示出来，就是FSM模型。

## 参考文献

1. > Wang, X., Bai, X., Ma, T., Liu, W., & Latecki, L. J. (2012). Fan shape model for object detection. In CVPR.