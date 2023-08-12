# 微分几何(Differential Geometry)

### 1.1 曲线(Curves)

我们考虑一个光滑的平面曲线,即嵌入在 $ IR^2 $中的可微1流形.这样一个曲线可以用参数化形式通过一个向量值函数 $x:[a,b]→IR^2$ 的形式表示, $x(u)=(x(u),y(u))^T,u∈[a,b]⊂IR^2$ (见第一章).坐标 x 和 y 假设是函数 $\mu$ 的可微函数.曲线在点 x($\mu$) 处的切向量 x′($\mu$) 为坐标函数的一阶导数,即 $x^′(\mu)=(x^′(\mu),y^′(\mu))^T$ .因为曲线定义为函数 x 的图像,因此可以通过不同的参数化得到相同的曲线.例如,$x_1(\mu)=(\mu,\mu)^T$和$x_2=(\mu^2,\mu^2)$描述$u\in[0,1]$ 的同一条曲线,们可以在不改变曲线形状的情况下改变参数化.利用重参数化**(Reparameterization)** $\nu(\mu)=\mu^2 $,我们获得 $x_1(\nu(\mu))=x_1(\mu^2)=x_2(\mu)$ .曲线的微分几何涉及曲线的特性,这些特性与特定的参数无关,如长度或曲率.

### 1.2 弧长(Arc Length)

在区间 [c,d]⊆[a,b] 内定义的任意曲线的长度 $l(c,d)$ 可以通过切向量的积分计算得到,即 $l(c,d)=\int_{1}^{2}||x^′(\mu)||d\mu $.因此,切向量 x′ 编码了曲线的**度量(Metric)**.参数曲线允许一个唯一的参数化,以便在参数区间和曲线之间通过使用重参数化来定义一个保持长度的映射,即**等距(Isometry)**, 重参数化公式为:
$$
s=s(\mu)=\int_{a}^{u}||x^′||dt \tag{3.1}
$$
此**弧长参数化(Arc length Parameterization)**与具体的曲线表示无关,并将参数区间 [a,b] 映射到 [0,L],其中 L=$l(a,b)$=$\int_{1}^{2}||x^′(\mu)||d\mu$ 是曲线的总长度.这个名称源于一个重要的性质:对于曲线上的任意点 x(s),曲线从 x(0) 到 x(s) 的长度等于 s .虽然任何正则曲线都可以用弧长参数化,但这种标准参数化一般不能用于表面.

### 1.3 **曲率(Curvature)**

假设一个正则曲线使用弧长参数化的话,我们可以定义一个点 x(s) 为:
$$
\kappa(s):=||x^{''}(s)||
$$
对于任意带参数化 $\mu$ 的正则曲线来说,我们可以根据弧长 $s(\mu)$ 重参数化来定义曲率.直觉上,曲率衡量的是曲线偏离直线的强度.换句话说,曲率是曲线切向量的导数和曲线法向量的关系,也可以用 x″(s)=$\kappa(s)n(s)$ 来定义.注意,在这个定义中,曲率是有符号的,因此当法线的方向颠倒时,曲率的符号就会改变.很容易看出来,一条直线的曲率是消失的,任何处处为零曲率的曲线一定是一条线段.常曲率的平面曲线是圆弧.曲率也可以定义为**密切圆(Osculation Circle)**半径的倒数.这个圆在 $x(\mu)$ 点处与曲线的局部最接近,可以这样构造:假设$c(\mu_{-},\mu,\mu_+)$ 为经过曲线上的三个点 $x(\mu_-) , x(\mu)$ 和 $x(\mu_+)$ 的一个圆,其中$\mu_-<\mu<\mu_+$ .则在点 $x(\mu)$ 处的密切圆 $c(\mu)$ 定义为 $c=\int_{\mu_-,\mu_+},→\mu(\mu_-,\mu,\mu_+) $.密切圆的半径为 $1/\kappa(u)$ ,与曲线在 $x(\mu)$ 处相切.

## 2 表面(Surfaces)

长度和曲率是曲线的欧几里德不变量;也就是说,它们在刚性运动下不会改变.现在我们来看看嵌入在 $IR^3$ 中的光滑表面的类似度量和曲率属性.使用表面的参数表示更容易定义这些属性,如下所述.然后,度量性质将从这个参数表示推导出来.

### 2.1 表面的参数化表示(Parametric Representation of Surfaces)

作为参数化表面表示的一个例子,我们来看看绘制世界地图的问题.如图3.1所示,问题是找到一种方法"展开"世界的表面,以获得一个2D平面.因为世界是封闭的,所以需要一个切口来切开它.例如,它可以沿着子午线切割,也就是一条连接两极的曲线.在展开过程中,请注意两个极点被拉伸成两条曲线.北极点转换为 AC 段,南极点转换为 BD 段.同时还可以注意到沿着球体的子午线已经被切成对应两段不同的曲线: AB 段和 CD 段.换句话说,如果一个城市恰好位于子午线上,则它会出现在地图的两侧.

![img](https://pic4.zhimg.com/80/v2-a772ece04b297bd7b6537802726cc163_720w.webp)

如图3.2所示,图上的每个点都可以提供两个坐标 (θ,φ) .在图3.1所示的映射中, 3D空间中的$(x,y,z)$坐标以及图中的 (θ,φ) 坐标通过以下方程连接起来,该方程称为球面参数方程:
$$
\mathrm{x}(\theta, \phi)=\left(\begin{array}{c}
x(\theta, \phi) \\
y(\theta, \phi) \\
z(\theta, \phi)
\end{array}\right)=\left(\begin{array}{c}
R \cos (\theta) \cos (\phi) \\
R \sin (\theta) \cos (\phi) \\
R \sin (\phi)
\end{array}\right)
$$
其中$ \theta∈[0,2\pi],\phi∈[−\pi/2,\pi/2]$ ,而 R为球体的半径.对于一般的表面我们将把这一映射表示为 $x(\mu.\nu)=(x(\mu,\nu),y(\mu,\nu),z(\mu.\nu))$ .

![img](https://pic4.zhimg.com/80/v2-7e9c4c87246f0ff6c6075a37f4382c33_720w.webp)

​																					图 2.球体坐标

对于参数方程,可以给出以下定义:

- 点 $p=(x,y,z)$ 处的坐标 $(\theta,\phi)$称为点 p 的**球体坐标(Spherical Coordinates)**.
- 图中每一条由**$θ=常数$**定义的垂直线,都对应于3D表面上的一条曲线,称为等θ曲线.在我们的例子中,等θ曲线是横贯球体两极的圆(即地球的子午线).
- 图中由φ=常数定义的每一条水平线都对应着一条等φ曲线.在我们的例子中,等φ曲线是地球的平行线,等φ对应的φ=0是赤道.

绘制等θ曲线和等φ曲线有助于我们理解一张地图应用到表面上是如何如何变形的.图中,等θ曲线和等φ曲线分别为垂直线和水平线,构成规则的网格.当地图应用到表面上时,可以看到网格在两极附近发生的扭曲.在这些区域上,这些网格的方形是高度扭曲的.在我们将曲率的概念从曲线推广到曲面之前,

### 2.2 度量属性(Metric Properties)

设连续表面 $S⊂\mathbb{R}^{3}$的参数形式如下:
$$
\mathrm{x}(u, v)=\left(\begin{array}{l}
x(u, v) \\
y(u, v) \\
z(u, v)
\end{array}\right),(u, v) \in \Omega \subset \mathbb{R}^{2}
$$
其中 $x,y $和 $z$是$\mu$和 $\nu$中的可微函数, $\Omega$是参数域.标量值 $(\mu,\nu)$ 是参数空间的坐标.

与曲线类似,表面的度量由函数 x 的一阶导数决定.如图3.3所示,在点$ x(\mu_0,\nu_0)∈S$ 的两个偏导数
$$
x_\mu(\mu_0,\nu_0):=\frac{\partial x}{\partial \mu}(\mu_0,\nu_0)
$$
And:
$$
x_\nu(\mu_0,\nu_0):=\frac{\partial x}{\partial \mu}(\mu_0,\nu_0)
$$
分别是两个等参数曲线:
$$
C_u(t)=x(\mu_0+t,\nu_0)
$$
And:
$$
C_v(t)=x(\mu_0,\nu_0+t)
$$
的切向量.下面我们去掉 $(\mu_0,\nu_0)$ 或 $(\mu,\nu)$ 以简化符号.但是要记住重要的一点,所有的量都是点式定义的,并且通常会在表面上变化.

![img](https://pic3.zhimg.com/v2-59eec637f5585c677911b82adf45086e_r.jpg)

​										图 3.将参数空间的切向量 $\bar{W}$转换为表面S 的切向量 w ,表面 $S$ 由参数化 x 描述.

假设一个**正则参数化(Regular Parameterization)**,即 $x_\mu × x_\nu≠0$ ,则 $S $的切平面是由两个切向量 $x_\mu$ 和 $x_\nu$ 展开而成的.表面法向量与两个切向量都正交,因此可以计算为:
$$
n=\frac{X_\mu\times X_\nu}{||X_{\mu}\times {X_\nu}||}
$$
外,我们可以定义 x 的任意方向导数.给定一个在参数空间下的方向向量$\bar{W}=(\mu_w,\nu_w)^T$,们考虑由 t 参数化的直线穿过 $(\mu_0,\nu_0)$并且方向为 $\bar{W}$,这由 $(\mu,\nu)=(\mu_0,\nu_0)+t\bar{W}$给出.穿过 x 的直线图像为曲线:
$$
C_w(t)=x(\mu_0+t\mu_w,\nu_0+t\nu_w)
$$
x 在点 $(\mu_0,\nu_0)$ 相对于方向 $\bar{W}$ 的方向导数 W定义为 $C_w(t)$ 在 t=0 处的切线, $W=\partial Cw(t)/\partial t $.通过运用链式法则,因此得到 $W=J\bar{W}$ ,其中 J 是 x 的**雅可比矩阵(Jacobian Matrix)**,定义为:
$$
J=\begin{bmatrix}
&\frac{\partial x}{\partial\mu} &\frac{\partial x}{\partial\nu}\\
&\frac{\partial y}{\partial\mu} &\frac{\partial y}{\partial\nu}\\
&\frac{\partial z}{\partial\mu} &\frac{\partial z}{\partial\nu}
\end{bmatrix}
$$
**第一基本形式(First Fundamental Form)**.参数化函数 x 的雅可比矩阵对应于将参数空间中的向量 $\bar{W}$变换为表面上的切向量 w 的线性映射.更通俗地说,雅可比矩阵编码了表面的度量,它允许测量角度,距离和面积是如何从参数域到曲面的映射进行变换的.设 $\bar{W_1}$ 和 $\bar{W_2}$ 是参数空间下的两个单位方向向量.这两个向量的夹角的余弦值可通过标量积$\bar{W_1}^T\bar{W_2}$ 得到.对应到表面的切线之间标量积为
$$
\bar{W_1}^T\bar{W_2}=(J\bar{W_1})^T(J\bar{W_2})=\bar{W_1}^T(J^TJ)\bar{W_2}
$$
矩阵 $J^TJ$也叫做 x 的**第一基本形式(First Fundamental Form)**,通常写成
$$
I=J^TJ=\begin{bmatrix}E&F\\F&G
\end{bmatrix}:=
\begin{bmatrix}
X^T_\mu X_\mu&X^T_\mu X_\nu\\
X^T_\mu X_\nu&X^T_\nu X_\nu
\end{bmatrix}

\tag{3.2}
$$
第一基本形式$I$定义了在$S$ 的切线空间下的一个内积.除了测量角度,我们还可以使用这一个内积来决定切线向量 w 的平方长度为 $||w||^2=\bar{w}^TI\bar{w}$.

这可以用来测量曲线 $x(t)=x(\mu(t))$的长度,曲线定义为在参数域中的正则曲线$u(t)=(u(t),v(t))$​图像.曲线的切向量可以通过链式法则得出:
$$
\frac{dx(u(t))}{dt}=\frac{\partial x}{\partial v} \frac{dv}{dt}=x_u u_t +x_v v_t
$$
因此,对于参数区间[a, b],我们可以使用方程(3.1)确定 $x(u(t))$ 的长度 $l(a,b)$,为:
$$
l(a,b)=\int^a_b\sqrt{(u_t.v_t)I(u_t.v_t)^T}dt=\int\sqrt{Eu_t^2+2Fu_tv_t+Gv_t^2dt}
$$
同理,我们可以测量特定参数区域$U\subseteq \Omega$对应的表面面积 A:
$$
A=\iint_U\sqrt{det(I)dudv}=\iint_U\sqrt{EG-F^2}dvdv
\tag{3.3}
$$
因为第一基本形式 $I$ 可以测量角度,距离和面积,因此它可以作为一个几何工具,有时也用 G 表示,称作**度量张量(Metric Tensor)**.

**各向异性(Anisotropy)**.利用雅可比矩阵,从参数空间位置$ (u_0,v_0) $传出的方向 $\bar{w}$可以通过参数化转换为切向量 w 如图4所示,也可以通过参数化 x 对一个小圆进行变换,得到一个小椭圆,称为**各向异性椭圆(Anisotropy Ellipse)**.考虑第一基本形式 $I$ 的特征向量 $\bar{e_1}$ 和 $\bar{e_2}$ ,以及与之相关的特征值$\lambda_1$和 $\lambda_2$ ,各向异性椭圆可表示为:

- 各向异性椭圆的轴分别为 $e_1=J\bar{e_1} $以及 $e_2=J\bar{e_2} $;
- 轴的长度分别为 $\sigma_1=\sqrt{\lambda_1}$ 以及 $\sigma_2=\sqrt{\lambda_2}$.

注意,轴的长度  $\sigma_1$ 和  $\sigma_2$ 也对应于雅可比矩阵 $J$ 的奇异值.

它们的表达式可以通过计算特征多项式的零值的平方根 $p(\sigma)=det(I−σId) $得到,其中 $Id$ 表示为如下的单位矩阵:
$$
\sigma_1=\sqrt{1/2(E+G)+\sqrt{(E-G)^2+4F^2}}\\
\sigma_2=\sqrt{1/2(E+G)-\sqrt{(E-G)^2+4F^2}}
$$
其中$ E , F ,G $表示第一基本形式 $ I$ 的系数(方程(3.2)).

![img](https://pic3.zhimg.com/80/v2-6909ab8744c51b55c1b94863aa54ce06_720w.webp)

​											图4 各向异性:参数空间下的小圆变换为笛卡尔空间下的小椭圆

### 2.3 表面曲率(Surface Curvature)

为了将曲率的概念从曲线扩展到曲面,我们来看看嵌在曲面中的曲线的曲率.设$ t=u_t x_u+v_t x_v $为表面点$ p∈S$的切向量,在参数空间下表示为 $\bar{t}=(u_t,v_t)^T .$在 p点的法曲率**(Normal Curvature)**为平面曲线的曲率,平面曲线是通过在**p**点创建一个跨过 **t** 和表面法线 **n** 且与表面相交的平面(见图3.5).我们可以将法曲率在  $\bar{t}$ 方向上表示为:
$$
\kappa_\eta(\bar{t})=\frac{\bar{t}^T II t}{\bar{t}^TIt}=\frac{eu^2_t+2fu_tv_t+gv_t^2}{Eu_t^2+2Fu_t v_t+Gv_t^2}
\tag{3.4}
$$
其中 $II$为**第二基本形式(Second Fundamental Form)**,表示为:
$$
II=J^TJ=\begin{bmatrix}e&f\\f&g
\end{bmatrix}:=
\begin{bmatrix}
x^T_{uu} n&x^T_{vv} n\\
x^T_{uv} n&x^T_{vv} n
\end{bmatrix}
$$
这里, $s$ 的二阶偏导表示为:
$$
x_{uu}:=\frac {\partial x}{\partial u^2},x_{uv}:=\frac{\partial ^2 x}{\partial u \partial v},x_{vv}:=\frac{\partial x}{\partial v^2}
$$
![img](https://pic1.zhimg.com/80/v2-07cf1a6cb894c85ff7ff416fa1fa3fc8_720w.webp)

- 图 5.表面与由跨过交点切向量和法向量的平面定义了一个法截面:嵌入在表面中的平面曲线.通过分析这一无限的曲率,我们可以定义表面的曲率.

表面的曲率特性可以通过考虑所有在 p 处法向截面的曲率来表示,即通过将切向量 t 绕表面法向旋转来表示.假设 $\kappa_n(\bar t)$ 随 $\bar t$的变化而变化,可以证明方程(3.4)的有理二次函数有两个显著的极值,称作**主曲率(Principal Curvatures)**.我们用 $\kappa_1$ 表示最大曲率, $\kappa_2$  表示最小曲率.

如果 $\kappa_1$≠$\kappa_2$ ,我们可以确定两个唯一的单位切向量 $t_1$ 和 $t_2$ ,叫**主方向(Principal Directions)**,它们分别与两个主曲率$\kappa_1$ 和 $\kappa_2$有关. $\kappa_1=\kappa_2$的表面点称为**脐点(Umbilical)**或**局部球(Locally Spherical)**.对于这样的点,其左右的切向量都可以认为是主方向,且曲率属性是各向同性的.例如,球面或平面上的每个点都是脐点,而由脐点组成的每一个连通面都一定包含在球面或平面中.

**欧拉定理(Euler Theorem)**.欧拉定理将法向曲线与主曲率联系起来:
$$
\kappa_n(\bar t)=\kappa_1 cos^2 \psi + \kappa_2sin^2\psi
$$
其中 $\psi$是 t 和 $t_1$的夹角.这一关系表明,表面的曲率完全由两个主曲率决定;任何法曲率都是最小曲率和最大曲率的凸组合.欧拉定理还表明,主方向总是互相正交的.这个性质可以用于如在四优重网格中计算曲率线网络(如第6章所述).对于所有的非脐点,这些曲线与两个唯一的主方向相切,因此在曲面上相交成直角.

**曲率张量(Curvature Tensor)**.曲面的局部性质可以用曲率张量 C 来描述,曲率张量 C 为一个具有特征值 $\kappa_1,\kappa_2$ , 0和相应特征向量 $t_1,t_2$ ,n的对称3 × 3矩阵.曲率张量可以构造为 $C=PDP^{-1}$ ,其中 $P=[t_1,t_2,n] , D=diag(\kappa_1,\kappa_2,0) .$

在本书中还广泛使用另外两种曲率测量方法:

-  **平均曲率(Mean Curvature)**H ,定义为主曲率的平均值:
  $$
  H=\frac{\kappa_1+\kappa_2}{2}
  \tag{3.5}
  $$
  
- **高斯曲率(Gaussian Curvature)**K ,定义为主曲率的乘积:

- $$
  K=\kappa_1 \kappa_2
  \tag{3.6}
  $$

高斯曲率可以将表面点分为三个不同的种类:**椭圆(Elliptical)**点( K>0 ),**双曲(Hyperbolic)**点( K<0 ),和**抛物(Parabolic)**点( K=0 ).双曲点处的表面局部呈鞍形,椭圆点处的表面局部呈凸形.抛物点通常位于分离椭圆和双曲区域的曲线上.高斯曲率和平均曲率通常用于表面的目视检查,如图.6所示.

![img](https://pic2.zhimg.com/80/v2-0a8b163ce00d7e762ac5e8c052474325_720w.webp)

-  图 36.彩色编码的曲率值,平均曲率(左)和高斯曲率(右).(图片来自于[Botsch et al. 06b]).

**内蕴几何(Intrinsic Geometry)**.在微分几何中,仅依赖于第一基本形式的性质(方程(3.2))称为内蕴性质.直观地说,一个表面的内在几何形状可以被生活在表面的2D生物感知到,而不需要知道第三维的信息.例子包括曲面上曲线的长度和角度.高斯著名的**绝妙定理(Egregium Theorema)**表明高斯曲率在局部等距下是不变的,因此对于表面来说也是独立的.因此高斯曲率可以直接从第一个基本形式确定.相反,平均曲率在等距下不是不变的,其变化取决于嵌入程度.注意,术语intrinsic也经常用来表示特定参数化的独立性.

**拉普拉斯算子(Laplace Operator)**.接下来的章节将广泛地使用**拉普拉斯算子(Laplace Operator)** Δ 和**拉普拉斯-贝尔特拉米(Laplace-Beltrami)**算子 $Δ_S $.一般来说,拉普拉斯算子定义为梯度散度,即, $Δ=∇^2=∇⋅∇ $.对于欧几里得空间中的双参数函数 $f(u,v)$ ,这个二阶微分算子可以写成二阶偏导数的和:
$$
\Delta f=\operatorname{div} \nabla f=\operatorname{div}\left(\begin{array}{l}
f_{u} \\
f_{v}
\end{array}\right)=f_{u u}+f_{v v}
$$
拉普拉斯-贝尔特拉米算子将这个概念扩展到定义在表面上的函数.对于定义在流形表面 S上的给定函数 $f$ ,拉普拉斯-贝尔特拉米定义为
$$
\Delta _S f=\operatorname{div}_S \nabla_S f
$$
其中需要在流形上定义合适的散度算子和梯度算子(详见[do Carmo 76]).将拉普拉斯-贝尔特拉米算子应用到表面的坐标函数 x 上,得到**平均曲率法向量(Mean Curvature Normal)**:
$$
\Delta_S \operatorname{x}=-2Hn
\tag{3.7}
$$
注意,尽管这个方程将拉普拉斯-贝尔特拉米算子与表面的(非内在)平均曲率联系起来,但算子本身是一种内蕴性质,只依赖于曲面的度规,即第一基本形式.为了简单起见,我们经常去掉下标,在上下文清晰的情况下,简单地使用符号 Δ 来表示拉普拉斯-贝尔特拉米运算子.

## 3 离散微分算子(Discrete Differential Operators)

在前一节中定义的微分性质要求曲面充分可微,例如,曲率的定义要求二阶导数的存在.由于多边形网格是分段线性曲面,上述概念不能直接应用.以下离散微分算子的定义是基于网格可以被解释为光滑表面的分段线性逼近的假设作为前提的.目标是直接从网格数据中计算出这个底层表面的微分特性的近似.近年来,人们提出了不同的方法.我们将关注拉普拉斯-贝尔特拉米算子事实上的标准离散化,并提供一个简短的推导结果公式,紧跟[Meyer et al.03].在[Pinkall and Polthier 93, Desbrun et al. 99]中提出了相同结果的替代推导.要了解更多细节,请参阅3.4节提供的参考文献和调查[Petitjean 02].

### 3.1 局部平均域(Local Averaging Region)

一般的想法是计算离散微分特性作为网格上点 x 的局部邻域 $\mathcal{N}(\mathbf{x})$的空间平均.通常 $\mathbf{x}$ 与网格顶点 $v_i$ 重合, $ n-$环邻域 $\mathcal{N}_n(v_i)$或局部测地线球作为平均域. 局部邻域的大小直接影响离散算子的稳定性和精度.邻域越大,平均运算引入的平滑程度越高,使得计算在有噪声的情况下更加稳定.对于干净的数据集,通常可选择较小的邻域,因为它们更准确地捕捉不同属性的细微变化.图3.7显示了顶点单环邻域上定义的三种平均域的变体.重心单元连接三角形重心与边缘中点.或者,我们可以定义一个局部Voronoi单元,将三角形重心替换为三角形的外圆心.Voronoi单元的紧密性导致了离散算子的严格误差边界,如[Meyer et al. 03]所示.然而,如图所示,外圆心可以在三角形的外面.虽然这并没有使下面的离散化失效,但通过确保局部平均区域建立一个完美的网格表面平铺,可以获得稍好的近似特性.这可以通过将钝角三角形的外圆心替换为与中心顶点相对的边的中点来实现.得到的平均面积表示为**混合Voronoi(Mixed Voronoi)**单元.

![img](https://pic3.zhimg.com/80/v2-7c557c2739e1b0b9ad75888ce3d18912_720w.webp)

- 7.蓝色表示用于计算与单环邻域的中心顶点相关联的离散微分算子的局部平均区域.

### 3.2 法向量(Normal Vectors)

几何处理和计算机图形学中的许多操作都需要法线,无论是每个面还是每个顶点;如Phong着色.单一三角形 $T=(x_i,x_j,x_k)$ 的法向量可以通过计算两条三角形边归一化的叉乘得到:
$$
\mathbf{n}(T)=\frac{\left(\mathbf{x}_{j}-\mathbf{x}_{i}\right) \times\left(\mathbf{x}_{k}-\mathbf{x}_{i}\right)}{\left\|\left(\mathbf{x}_{j}-\mathbf{x}_{i}\right) \times\left(\mathbf{x}_{k}-\mathbf{x}_{i}\right)\right\|}
$$
将顶点法线计算为局部单环邻域法向量的空间平均值,可以得到附属三角形(常数)法向量的归一化加权平均值:
$$
\mathbf{n}(v)=\frac{\sum_{T \in \mathcal{N}_{1}(v)} \alpha_{T} \mathbf{n}(\mathbf{T})}{\left\|\sum_{T \in \mathcal{N}_{1}(v)} \alpha_{T} \mathbf{n}(\mathbf{T})\right\|}
$$
对于权重$\alpha_T$ ,有多种可选方向.以下我们描述最常用的方法,并在图8中对它们进行比较:

- 常数权重 $\alpha_T$=1便于计算,但是没有考虑边长,三角形面积或者角度,因此对于不规则的网格,其结果是反直觉的.
- 图3.7所示的局部平均域建议基于三角形面积的加权,即 $\alpha_T=|T|$ .这种计算方法特别高效,因为基于面积加权的面法线仅仅是两条三角形边的叉乘(非归一化).但是同样也会出现反直觉的结果.
- 在足够小的测地线圆盘上进行平均,相当于通过附属三角形角 $\alpha_T=\Theta_T$ 来加权(见图3.10).由于引入了三角函数,使得这种方法计算成本更高,但一般来说,它可以给出更好的结果.

对于大多数应用,基于角加权的面法线提供了计算效率和准确性之间的一个很好的权衡.更多的细节和不同方法的比较可以在[Max 99,Jin et al. 05]中找到.

![img](https://pic2.zhimg.com/80/v2-38da7eb8fd6541d2466c66622752fe9d_1440w.webp)

- 8. 在同一个均匀细分圆柱上计算逐顶点法线的不同方法:中间为常数权重和面积权重的结果,右边为角权重.

### 3.3 梯度(Gradients)

由于拉普拉斯-贝尔特拉米算子定义为梯度的散度,我们首先来看看一个分段线性三角形网格的函数梯度的合适定义.梯度在网格参数化(第五章)和变形(第九章)中也起着重要的作用.

我们假设分段线性函数 $f$在每一个网格顶点处为 $f(v_i)=f(x_i)=f(u_i)=f_i$ ,且每一个三角形 $(x_i,x_j,x_k) $内线性插值:
$$
f(\mathbf{u})=f_iB_i(\mathbf{u})+f_jB_j(\mathbf{u})+f_kB_k(\mathbf{u})
$$
其中 $\mathbf{u}=(u,v)$为在三角形中2D保角参数化的表面点 $\mathbf{x}$ 的参数对(见第五章).图.9给出了用于插值的线性重心基函数.

$f$ 的梯度为:$\Delta f(\mathbf{u})=f_i \Delta B_i(\mathbf{u})+f_j \Delta B_j(\mathbf{u})+f_k \Delta B_k(\mathbf{u})$

由于基函数满足单位分割的重心条件,即对于所有的 $\mathbf{u}$ 满足
$$
\nabla B_i(\mathbf{u})+\nabla B_j(\mathbf{u})+\nabla B_k(\mathbf{u})=1\\
所以基函数的和为0,即\\
\nabla B_i(\mathbf{u})+\nabla B_j(\mathbf{u})+\nabla B_k(\mathbf{u})=0\\
因此上述方程可以写成\\
\nabla f(\mathbf{u})=(f_j-f_i)\nabla B_j(\mathbf{u})+(f_k-f_i)\nabla B_k(\mathbf{u})
$$
如图9所述,基函数最陡的上升方向与对应顶点的对边正交.通过适当的归一化, $B_i$ 的梯度为
$$
\nabla B_i(\mathbf{u})=\frac{(x_k-x_j)^{\bot}}{2A_T}
\tag{3.8}
$$
其中 ⊥ 表示在三角形平面内逆时针旋转90度,$A_T$ 为三角形 T 的面积.因此,在三角形 T 内分段线性函数 $f$ 的梯度值为常数
$$
\nabla{f}(\mathbf{u})=(f_j-f_i)\frac{(x_i-x_k)^{\bot}}{2A_T}+(f_k-f_i)\frac{(x_j-x_i)^{\bot}}{2A_T}
\tag{3.9}
$$
![img](https://pic4.zhimg.com/80/v2-d04d4b99e650dbf3c491bcd69e18d927_1440w.webp)

- 图 9. 三角形上重心插值的线性基函数

### 3.4 离散拉普拉斯-贝尔特拉米算子(Discrete Laplace-Beltrami Operator)

我们讨论拉普拉斯-贝尔特拉米算子的两种离散:均匀图拉普拉斯和广泛使用的余切公式.

**均匀拉普拉斯(Uniform Laplacian)**.Taubin [Taubin 95]提出了拉普拉斯-贝尔特拉米算子的均匀离散化
$$
\Delta{f(v_i)}=\frac{1}{\mathcal{N}_1(v_i)}\sum_{v_j \in \mathcal{N}_1(v_i)}(f_j-f_i)
\tag{3.10}
$$
其中总数是所有单环邻域 $v_j \in \mathcal{N}_1(v_i)$的总和.应用到坐标函数 X 中,均匀图拉普拉斯 $\Delta x_i$求出从中心顶点 $x_i$ 到单环顶点$x_j$ 的平均值的向量.虽然计算简单且高效,但得到的向量即使对于顶点的平面构型也可以是非零的.然而,在这种情况下,我们期望拉普拉斯函数为零,因为整个网格区域的平均曲率为零(c.f. Equation(3.7)).这表明对于非均匀网格,均匀拉普拉斯算子不是一种合适的离散化方法.事实上,由于均匀拉普拉斯算子的定义只依赖于网格的连通性,所以其完全不适应顶点的空间分布.虽然这在许多应用中是不利的,但我们在第4章和第6章中讨论了如何利用这种嵌入的不变性来改善各向同性网格中顶点的局部分布.

**余切公式(Cotangent formula)**.拉普拉斯-贝尔特拉米算子可以通过混合有限元/有限体积法得到更精确的离散化[Meyer et al.03].目标是在局部平均域 $A_i=A(v_i)$ 上积分分段线性函数梯度的散度.为了简化积分,我们利用向量值函数 F 的散度定理:
$$
\int_{A_i} \mathbf{divF(u) dA}=\int_{\partial A_i}\mathbf{F(u)\cdot n(u)ds}
$$
这个方程将平均域$A_i$ 与沿 $A_i$ 的边界 $\partial A_i$ 的积分联系起来,其中 $\mathbf{n}$ 是边界上向外的单位法线(见图.10).应用到拉普拉斯方程中,得到
$$
\int_{A_i}\Delta \mathbf{f(u) dA}=\int_{\partial A_i}\mathbf{div \nabla dA}=\int _{\partial A_i}\nabla \mathbf{f(u)\cdot n(u)ds}
$$
我们对每个三角形分别进行积分来分割这个积分.由于Voronoi局部区域的边界经过两条三角形边的中点a和b(见图3.10(右)),以及 $\nabla f(x)$ 在每个三角形内是常数,因此对于一个三角形T 的积分可以计算为
$$
\int_{\partial A_i \cap T }  \mathbf{\nabla f(u)\cdot n(u)ds}=\nabla \mathbf{f(u)\cdot{(a-b)^\bot}}=\frac{1}{2}\mathbf{\nabla f(u)\cdot(x_j-x_k)^{\bot}}
$$
代入到方程(3.9)为
$$
\int_{A_i}\Delta \mathbf{f(u)\cdot {n(u)} ds}=(f_j-f_i) \mathbf{\frac{(x_i-x_k)^{\bot}{\cdot}(x_j-x_k)^{\bot}}{4A_T}}+(f_k-f_i) \mathbf{\frac{(x_i-x_i)^{\bot}{\cdot}(x_j-x_k)^{\bot}}{4A_T}}
$$
![img](https://pic1.zhimg.com/80/v2-d19d9029f94abf26f6fdf7212ca9289c_1440w.webp)

图 10.在离散拉普拉斯-贝尔特拉米算子和离散高斯曲率算子的推导中使用的量的阐述.

设 $\gamma_j , \gamma_k$ 分别为三角形顶点 $v_j,v_k$​ 的内角.由于
$$
A_T=\frac{1}{2}\mathbf{sin \gamma_j ||x_j-x_i||||x_j-x_k||=\frac{1}{2}sin\gamma _{k}||x_i-x_k||||x_j-x_k||},\\And:\\
\mathbf{cos\gamma_j=\frac{(x_j-x_i)(x_j-x_k)}{||x_j-x_i||||x_j-x_k||}},\mathbf{cos\gamma_k=\frac{(x_i-x_k)(x_j-x_k)}{||x_i-x_k||||x_j-x_k||}}\\简化为
=>\mathbf{\int_{\partial A_i \cap T }  \mathbf{\nabla f(u)\cdot n(u)ds}=\frac{1}{2}(cot\gamma_k(f_j-f_i))+cot\gamma_j(f_k-f_i)}
$$
因此当对整个平均区域 $A_i$ 进行积分时我们就得到:
$$
\mathbf{\int_{A_i}\Delta \mathbf{f(u)dA}=\frac{1}{2}\sum_{v_j \in \mathcal{N}_1(v_i)}(cot\alpha_{i,j}+cot\beta_{i,j})(f_j-f_i)}
$$
其中我们如图10所示重新标记角度.因此,函数 $f$ 在顶点 $v_i$ 处的拉普拉斯-贝尔特拉米算子的离散平均为
$$
\mathbf{\Delta \mathbf{f(v_i)}:=\frac{1}{2A_i}\sum_{v_j \in \mathcal{N}}(cot\alpha_{i,j}+cot\beta_{i,j})(f_j-f_i)}
\tag{3.11}
$$
方程(3.11)可能是计算机图形学中最广泛使用的拉普拉斯-贝尔特拉米算子的离散化,通常用于各种几何处理任务,如表面平滑(第4章),参数化(第5章)和外形建模(第9章).然而,余切离散化也有一些缺点.如果 $\alpha_{i,j}+\beta_{i,j}>\pi$则余切权重 ($cot\alpha_{i,j}+cot\beta_{i,j}$) 变成了负数.这可能在某些应用中导致三角形翻转,例如,在计算参数化时就会出现(见第5章).此外,方程(3.11)的离散拉普拉斯-贝尔特拉米也不是纯粹内蕴的,即它的评估会导致不同的结果,即使是对于两个具有不同三角剖分的等值面来说也一样.我们参考3.4节中的一些另外的拉普拉斯-贝尔特拉米的离散定义来解决这些缺点.

由于拉普拉斯算子被定义为梯度的散度,为了完整起见,我们简要描述**散度算子(Divergence Operator)**[Tong et al. 03].假设一个向量场 $\mathbf{w:S \rightarrow \mathbb{R}^3 }$是由每个三角形 $T$ 的恒定向量 $W_T$ 定义的(如,分段线性函数 $f$的梯度).离散散度计算从向量场中的每个顶点 $v_i$ 到其附属三角形$T \in \mathcal{N}(v_i)  $的标量值 $div\mathbf{ w(v_i)}$:
$$
div\mathbf{ w(v_i)}=\frac{1}{A_i}\sum_{T \in \mathcal{N}_1(v_i)} \nabla B{_i}|{_T}\cdot \mathbf{W_T A_T}
\tag{3.12}
$$
其中$\nabla B{_i}|{_T}$ 为为三角形 $T$中顶点 $v_i$ 的基函数的(常数)梯度向量(见方程(3.8)).请注意,散度(3.12),梯度(3.9)和拉普拉斯(3.11)的离散性是一致的, $\mathbf{\Delta f=div \nabla f}$在离散情况下也成立.

### 3.5 离散曲率(Discrete Curvature)

当应用于坐标函数 $\mathbf{x}$ 时,拉普拉斯-贝尔特拉米算子提供了平均曲率法线的离散近似(见方程(3.17)).因此,我们可以定义在顶点$v_i$ 处的绝对离散平均曲率为:
$$
H(v_i)=\frac{1}{2}||\mathbf{\Delta x_i}||
\tag{3.13}
$$
Meyer等人[Meyer et al. 03]也提出了高斯曲率离散算子的推导:
$$
K(v_i)=\frac{1}{A_i}(2\pi-\sum_{v_j \in \mathcal{N}_1(v_i)}\Theta_j)
\tag{3.14}
$$
其中 $\Theta_j$表示在顶点 $v_i$ 处附属三角形的角度(见图3.10).这个公式是Gauss-Bonnet定理的直接结果[do Carmo 76].根据平均曲率(3.13)和高斯曲率(3.14)的离散近似,主曲率可由方程(3.6)和方程(3.5)求得:
$$
\kappa_{1,2}(v_i)=H(v_i)\pm \sqrt{H(v_i)^2-K(v_i)}
$$

### 3.6 离散曲率张量(Discrete Curvature Tensor)

与大量拉普拉斯-贝尔特拉米算子的离散版本类似,市面上也有许多用于直接估计多边形表面上的曲率张量的方法(见3.4节的参考文献).我们简要描述了由Cohen-Steiner和Morvan提出的方法[Cohen-Steiner和Morvan 03],该方法已成功地应用于表面重网格[Alliez et al. 03a]和曲率域形状处理[Eigensatz et al. 08].在[Hilde- brandt和Polthier 04]中提出了一个类似的定义.

其基本思想是为每条边定义一个曲率张量,方法是沿着这条边指定一个最小曲率为0,根据这条边的二面角指定一个最大曲率.对局部邻域 $A(v)$求平均,得到与 $A(v)$相交的边的一个简单的求和公式:
$$
C(v)=\frac{1}{A(v)}\sum_{e\in A(v)}\beta(e)||e \cap A(v)||\bar{e}\bar{e}^T
$$
其中 $\beta(e)$ 为边 $e$ 的两个附属面的法线之间的带符号二面角, $||e \cap A(v)||$为 $A(v)$ 中包含的 $e$ 的部分长度,且 $\mathbf{\bar{e}=\frac{e}{||e||}}$ .局部邻域$A(v)$ 通常可选为顶点 $v$ 的单环或二环,但也可计算为一个局部测地线盘,即网格上所有与v相隔 $v$ 一定(测地线)距离内的点.这可能更适合于非均匀细分表面,其中 $n$-环邻域 $\mathcal{N}_n(v)$ 的大小会在网格上显著地变化.正如在[Rusinkiewicz 04]中指出的,张量平均对于低价顶点和小(例如,单环)邻域会产生不准确的结果.

## 3.4 总结和延伸阅读

通过离散来模拟光滑表面的微分性质是多年来一个活跃的研究领域.Pinkall和Polthier讨论了离散的最小曲面,并提出了使用网格上Dirichlet能量最小化方程(3.11)的推导[Pinkall和Polthier 93].Bobenko和Springborn [Bobenko and Springborn 07]在模拟表面的内蕴Delaunay三角剖分上评估方程(3.11),这使得评估可以独立于网格的特定类型.Zayer等人[Zayer et al. 05b]将余切权重替换为正平均值坐标[Floater 03],并对圆形区域进行积分,而不是对Voronoi区域进行积分.虽然这导致了拉普拉斯-贝尔特拉米离散化的不精确,但它避免了负权值.在[Hildebrandt et al. 06]中给出了离散几何性质的收敛条件的系统研究.

另一种评估局部表面属性的方法是使用局部的高阶重建表面,然后在重建的表面面片上分析评估所需的属性.局部表面面片,通常是低度的二元多项式,被拟合到样本点[Cazals and Pouget 03, Petitjean 02, Welch and Witkin 94]和局部邻域内大概吻合的法线上[Goldfeather and Interrante 04].

Rusinkiewicz提出了一种用顶点法线的有限差分逼近曲率的方案[Rusinkiewicz 04].Theisel等人的一种相关方法[Theisel et al.04]考虑了分段线性表面和分段线性法线场.

Grinspun等人提供了离散微分几何不同概念和应用的广泛概述.特别是,他们提出了一种基于离散外部微积分来定义离散微分算子的替代方法[Grinspun et al. 08].Wardetzky等人根据一组从光滑设置中得到的理想属性对最常见的离散拉普拉斯算子进行分类[Wardetzky et al. 07].它们表明离散算子不能同时满足所有可识别的性质,如对称性,局部性,线性精度和正性.例如,方程(3.11)的余切公式满足前三个性质,但不满足第四个性质,因为边权值可以设为负值.因此,离散化的算法选择取决于具体的应用场景.
