---
layout: post
title: "麦轮底盘运动解算"
description: "麦轮底盘运动解算"
categories: [note]
tags: [Mecanum]
redirect_from:
  - /2020/02/03/
---

> Mecanum Wheel

* Kramdown table of contents
{:toc .toc}
## 麦轮底盘运动解算

部分抄袭来源：https://blog.csdn.net/banzhuan133/article/details/69229922

### 一、安装方式

![1579249426723](C:\Users\Liu Chuanzheng\AppData\Roaming\Typora\typora-user-images\1579249426723.png)

我们的机器人底盘麦轮安装方式为**O-长方形**：轮子转动可以产生 yaw 轴转动力矩，而且转动力矩的力臂也比较长。是最常见的安装方式。

### 二、传统解算过程

①将底盘的运动分解为三个独立变量来描述；

②根据第一步的结果，计算出每个轮子轴心位置的速度；

③根据第二步的结果，计算出每个轮子与地面接触的辊子的速度；

④根据第三部的结果，计算出轮子的真实转速。

#### 一、底盘运动的分解

刚体在平面内的运动可以分解为三个独立分量：X轴平动、Y轴平动、yaw 轴自转。如下图所示，底盘的运动也可以分解为三个量：

![v_{t_x}](http://zhihu.com/equation?tex=v_%7Bt_x%7D) 表示 X 轴运动的速度，即左右方向，定义向右为正；

![v_{t_y}](http://zhihu.com/equation?tex=v_%7Bt_y%7D) 表示 Y 轴运动的速度，即前后方向，定义向前为正；

![\vec{\omega}](http://zhihu.com/equation?tex=+%5Cvec%7B%5Comega%7D) 表示 yaw 轴自转的角速度，定义逆时针为正。

以上三个量一般都视为四个轮子的几何中心（矩形的对角线交点）的速度。

![img](https://img-blog.csdn.net/20170405153514234?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmFuemh1YW4xMzM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

#### 二、计算出轮子轴心位置的速度

定义：

![\vec{r}](http://zhihu.com/equation?tex=%5Cvec%7Br%7D) 为从几何中心指向轮子轴心的矢量；

![\vec{v}](http://zhihu.com/equation?tex=%5Cvec%7Bv%7D) 为轮子轴心的运动速度矢量；

![\vec{v_r}](http://zhihu.com/equation?tex=%5Cvec%7Bv_r%7D) 为轮子轴心沿垂直于 ![\vec{r}](http://zhihu.com/equation?tex=%5Cvec%7Br%7D) 的方向（即切线方向）的速度分量；

那么可以计算出：

![\vec{v}=\vec{v_t}+\vec{\omega} \times \vec{r}](http://zhihu.com/equation?tex=%5Cvec%7Bv%7D%3D%5Cvec%7Bv_t%7D%2B%5Cvec%7B%5Comega%7D+%5Ctimes+%5Cvec%7Br%7D)

分别计算 X、Y 轴的分量为：

![\begin{equation}\begin{cases}v_x=v_{t_x}-\omega \cdot r_y \\v_y=v_{t_y}+\omega \cdot r_x \\\end{cases}\end{equation}](http://zhihu.com/equation?tex=%5Cbegin%7Bequation%7D%0A%5Cbegin%7Bcases%7D%0Av_x%3Dv_%7Bt_x%7D-%5Comega+%5Ccdot+r_y+%5C%5C%0Av_y%3Dv_%7Bt_y%7D%2B%5Comega+%5Ccdot+r_x+%5C%5C%0A%5Cend%7Bcases%7D%0A%5Cend%7Bequation%7D)

同理可以算出其他三个轮子轴心的速度。

**![img](https://img-blog.csdn.net/20170405153524139?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmFuemh1YW4xMzM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)**

#### 三、计算辊子的速度

根据轮子轴心的速度，可以分解出沿辊子方向的速度 ![\vec{v_\|}](http://zhihu.com/equation?tex=%5Cvec%7Bv_%5C%7C%7D) 和垂直于辊子方向的速度 ![\vec{v_\perp}](http://zhihu.com/equation?tex=%5Cvec%7Bv_%5Cperp%7D) 。其中 ![\vec{v_\perp}](http://zhihu.com/equation?tex=+%5Cvec%7Bv_%5Cperp%7D) 是可以无视的

![\vec{v_\|}=\vec{v} \cdot \hat{u}=(v_x\hat{i}+v_y\hat{j})\cdot(-\frac{1}{\sqrt{2}}\hat{i}+\frac{1}{\sqrt{2}}\hat{j})=-\frac{1}{\sqrt{2}}v_x+\frac{1}{\sqrt{2}}v_y](http://zhihu.com/equation?tex=%5Cvec%7Bv_%5C%7C%7D%3D%5Cvec%7Bv%7D+%5Ccdot+%5Chat%7Bu%7D%3D%28v_x%5Chat%7Bi%7D%2Bv_y%5Chat%7Bj%7D%29%5Ccdot%28-%5Cfrac%7B1%7D%7B%5Csqrt%7B2%7D%7D%5Chat%7Bi%7D%2B%5Cfrac%7B1%7D%7B%5Csqrt%7B2%7D%7D%5Chat%7Bj%7D%29%3D-%5Cfrac%7B1%7D%7B%5Csqrt%7B2%7D%7Dv_x%2B%5Cfrac%7B1%7D%7B%5Csqrt%7B2%7D%7Dv_y)

**![img](https://img-blog.csdn.net/20170405153530406?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmFuemh1YW4xMzM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)**

#### 四、计算轮子的速度

从辊子速度到轮子转速的计算比较简单：

![v_w=\frac{v_\|}{cos 45^\circ}=\sqrt{2}(-\frac{1}{\sqrt{2}}v_x+\frac{1}{\sqrt{2}}v_y)=-v_x+v_y](http://zhihu.com/equation?tex=v_w%3D%5Cfrac%7Bv_%5C%7C%7D%7Bcos+45%5E%5Ccirc%7D%3D%5Csqrt%7B2%7D%28-%5Cfrac%7B1%7D%7B%5Csqrt%7B2%7D%7Dv_x%2B%5Cfrac%7B1%7D%7B%5Csqrt%7B2%7D%7Dv_y%29%3D-v_x%2Bv_y)

![img](https://img-blog.csdn.net/20170405153535967?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmFuemh1YW4xMzM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

根据上图所示的 ![a](http://zhihu.com/equation?tex=a) 和 ![b](http://zhihu.com/equation?tex=b) 的定义，有

![\begin{equation}\begin{cases}v_x=v_{t_x}+\omega b \\v_y=v_{t_y}-\omega a \\\end{cases}\end{equation}](http://zhihu.com/equation?tex=%5Cbegin%7Bequation%7D%0A%5Cbegin%7Bcases%7D%0Av_x%3Dv_%7Bt_x%7D%2B%5Comega+b+%5C%5C%0Av_y%3Dv_%7Bt_y%7D-%5Comega+a+%5C%5C%0A%5Cend%7Bcases%7D%0A%5Cend%7Bequation%7D)

结合以上四个步骤，可以根据底盘运动状态解算出四个轮子的转速：

![\begin{equation}\begin{cases}v_{w_1}=v_{t_y}-v_{t_x}+\omega(a+b) \\v_{w_2}=v_{t_y}+v_{t_x}-\omega(a+b) \\v_{w_3}=v_{t_y}-v_{t_x}-\omega(a+b) \\v_{w_4}=v_{t_y}+v_{t_x}+\omega(a+b) \\\end{cases}\end{equation}](http://zhihu.com/equation?tex=%5Cbegin%7Bequation%7D%0A%5Cbegin%7Bcases%7D%0Av_%7Bw_1%7D%3Dv_%7Bt_y%7D-v_%7Bt_x%7D%2B%5Comega%28a%2Bb%29+%5C%5C%0Av_%7Bw_2%7D%3Dv_%7Bt_y%7D%2Bv_%7Bt_x%7D-%5Comega%28a%2Bb%29+%5C%5C%0Av_%7Bw_3%7D%3Dv_%7Bt_y%7D-v_%7Bt_x%7D-%5Comega%28a%2Bb%29+%5C%5C%0Av_%7Bw_4%7D%3Dv_%7Bt_y%7D%2Bv_%7Bt_x%7D%2B%5Comega%28a%2Bb%29+%5C%5C%0A%5Cend%7Bcases%7D%0A%5Cend%7Bequation%7D)

### 三、另一种简单计算方式

「传统」的推导过程虽然严谨，但还是比较繁琐的。这里介绍一种简单的逆运动学计算方式。

全向移动底盘是一个纯线性系统，而刚体运动又可以线性分解为三个分量。那么只需要计算出麦轮底盘在「沿X轴平移」、「沿Y轴平移」、「绕几何中心自转」时，四个轮子的速度，就可以通过简单的加法，计算出这三种简单运动所合成的「平动+旋转」运动时所需要的四个轮子的转速。而这三种简单运动时，四个轮子的速度可以通过简单的测试，或是推动底盘观察现象得出。

当底盘沿着 X 轴平移时：

![\begin{equation}\begin{cases}v_{w_1}=-v_{t_x} \\v_{w_2}=+v_{t_x} \\v_{w_3}=-v_{t_x} \\v_{w_4}=+v_{t_x} \\\end{cases}\end{equation}](http://zhihu.com/equation?tex=%5Cbegin%7Bequation%7D%0A%5Cbegin%7Bcases%7D%0Av_%7Bw_1%7D%3D-v_%7Bt_x%7D+%5C%5C%0Av_%7Bw_2%7D%3D%2Bv_%7Bt_x%7D+%5C%5C%0Av_%7Bw_3%7D%3D-v_%7Bt_x%7D+%5C%5C%0Av_%7Bw_4%7D%3D%2Bv_%7Bt_x%7D+%5C%5C%0A%5Cend%7Bcases%7D%0A%5Cend%7Bequation%7D)
![\begin{equation}\begin{cases}v_{w_1}=v_{t_y} \\v_{w_2}=v_{t_y} \\v_{w_3}=v_{t_y} \\v_{w_4}=v_{t_y} \\\end{cases}\end{equation}](http://zhihu.com/equation?tex=%5Cbegin%7Bequation%7D%0A%5Cbegin%7Bcases%7D%0Av_%7Bw_1%7D%3Dv_%7Bt_y%7D+%5C%5C%0Av_%7Bw_2%7D%3Dv_%7Bt_y%7D+%5C%5C%0Av_%7Bw_3%7D%3Dv_%7Bt_y%7D+%5C%5C%0Av_%7Bw_4%7D%3Dv_%7Bt_y%7D+%5C%5C%0A%5Cend%7Bcases%7D%0A%5Cend%7Bequation%7D)

当底盘绕几何中心自转时：

![\begin{equation}\begin{cases}v_{w_1}=+\omega(a+b) \\v_{w_2}=-\omega(a+b) \\v_{w_3}=-\omega(a+b) \\v_{w_4}=+\omega(a+b) \\\end{cases}\end{equation}](http://zhihu.com/equation?tex=%5Cbegin%7Bequation%7D%0A%5Cbegin%7Bcases%7D%0Av_%7Bw_1%7D%3D%2B%5Comega%28a%2Bb%29+%5C%5C%0Av_%7Bw_2%7D%3D-%5Comega%28a%2Bb%29+%5C%5C%0Av_%7Bw_3%7D%3D-%5Comega%28a%2Bb%29+%5C%5C%0Av_%7Bw_4%7D%3D%2B%5Comega%28a%2Bb%29+%5C%5C%0A%5Cend%7Bcases%7D%0A%5Cend%7Bequation%7D)

### 四、解算代码

```c
//结构体内相关的需要初始化的量，代码移植时需要注意与机械组沟通，修改尺寸
pchassis->wheel_perimeter=152*PI;  //麦轮周长
pchassis->wheeltrack=430;   //左右轮车轴距离
pchassis->wheelbase=360;   //前后轮车轴距离

//底盘速度分解到四个轮子上	
void mecanum_calculate(void)
{
	static float rotate_ratio_fr;//前右轮 
	static float rotate_ratio_fl;//前左轮
	static float rotate_ratio_bl;//后左轮
	static float rotate_ratio_br;//后右轮
	static float wheel_rpm_ratio;

    //pchassis->rotate_x_offset和pchassis->rotate_y_offset为X轴和Y轴偏移量，编码器与陀螺仪数据融合时会用到，否则置0；57.3f为180/PI，用于角度和弧度单位转换
	rotate_ratio_fr = ((pchassis->wheelbase + pchassis->wheeltrack) / 2.0f - pchassis->rotate_x_offset + pchassis->rotate_y_offset) / 57.3f;  //右前轮，单位为mm*rad/deg
	rotate_ratio_fl = ((pchassis->wheelbase + pchassis->wheeltrack) / 2.0f - pchassis->rotate_x_offset - pchassis->rotate_y_offset) / 57.3f;  //左前轮，单位为mm*rad/deg
	rotate_ratio_bl = ((pchassis->wheelbase + pchassis->wheeltrack) / 2.0f + pchassis->rotate_x_offset - pchassis->rotate_y_offset) / 57.3f;  //左后轮，单位为mm*rad/deg
	rotate_ratio_br = ((pchassis->wheelbase + pchassis->wheeltrack) / 2.0f + pchassis->rotate_x_offset + pchassis->rotate_y_offset) / 57.3f;  //右后轮，单位为mm*rad/deg

    //MOTOR_DECELE_RATIO为减速比，根据3508电机的减速箱的减速比而改变，手册上为3591/187
	wheel_rpm_ratio = 60.0f / (pchassis->wheel_perimeter * MOTOR_DECELE_RATIO);  //单位为r/(min*mm)

    //限制正反向最大值
	MEC_VAL_LIMIT(pchassis->xv_mm_s, -MAX_CHASSIS_VX_SPEED, MAX_CHASSIS_VX_SPEED); //mm/s
	MEC_VAL_LIMIT(pchassis->yv_mm_s, -MAX_CHASSIS_VY_SPEED, MAX_CHASSIS_VY_SPEED); //mm/s
	MEC_VAL_LIMIT(pchassis->zw_deg_s, -MAX_CHASSIS_VW_SPEED, MAX_CHASSIS_VW_SPEED); 	//deg/s

	float wheel_rpm[4];
	float max = 0;

    //X轴平移、Y轴平移、绕YAW轴旋转三种运动合成，并将单位转换为rpm
	wheel_rpm[0] = (pchassis->xv_mm_s - pchassis->yv_mm_s + pchassis->zw_deg_s * rotate_ratio_fr) * wheel_rpm_ratio;
	wheel_rpm[1] = (pchassis->xv_mm_s + pchassis->yv_mm_s + pchassis->zw_deg_s * rotate_ratio_fl) * wheel_rpm_ratio;
	wheel_rpm[2] = (-pchassis->xv_mm_s + pchassis->yv_mm_s + pchassis->zw_deg_s * rotate_ratio_bl) * wheel_rpm_ratio;
	wheel_rpm[3] = (-pchassis->xv_mm_s - pchassis->yv_mm_s + pchassis->zw_deg_s * rotate_ratio_br) * wheel_rpm_ratio;

    //下面两个函数为限制输出
	//find max item
	for (uint8_t i = 0; i < 4; i++)
	{
		if (fabs(wheel_rpm[i]) > max)
			max = fabs(wheel_rpm[i]);
	}
	//equal proportion
	if (max > MAX_WHEEL_RPM)
	{
		float rate = MAX_WHEEL_RPM / max;
		for (uint8_t i = 0; i < 4; i++)
			wheel_rpm[i] *= rate;
	}
	
	pchassis->wheel_rpm[0]=wheel_rpm[0];
	pchassis->wheel_rpm[1]=wheel_rpm[1];
	pchassis->wheel_rpm[2]=wheel_rpm[2];
	pchassis->wheel_rpm[3]=wheel_rpm[3];	
}
```

```c
//一些常量的宏定义，移植代码时要根据实际情况修改
//修改！！！！！！！！！！！！！！！！！！！！！
#define MOTOR_DECELE_RATIO (1.0f / 19.0f)
#define HIGH_RmeoteSpeed		660
#define MEC_VAL_LIMIT(val, min, max) \
  do                                 \
  {                                  \
    if ((val) <= (min))              \
    {                                \
      (val) = (min);                 \
    }                                \
    else if ((val) >= (max))         \
    {                                \
      (val) = (max);                 \
    }                                \
  } while (0);
  
#define MAX_CHASSIS_VX_SPEED 3000 
#define MAX_CHASSIS_VY_SPEED 3000
#define MAX_CHASSIS_VW_SPEED 300 
#define INIT_current 3000 	
#define MAX_WHEEL_RPM 9000 //9000rpm = 3732mm/s
#define PI 3.14159265354f
```







# 仅限MSE Star团队内部参考！！！

![1579606591621](C:\Users\Liu Chuanzheng\AppData\Roaming\Typora\typora-user-images\1579606591621.png)

