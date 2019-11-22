---
title: markdown 之 LaTeX 特殊公式格式笔记
date:  19-3-8 17:34 +08
tags:  markdown,转载
copied: https://blog.csdn.net/thither_shore/article/details/52260742
latex: true
---

>转载自[ThitherShore](https://blog.csdn.net/thither_shore/article/details/52260742)

### 分数导致字母太小  
在LaTeX中若用`\frac`有时会导致字母显示出来很小，解决方案是使用`\dfrac`,即`\displaystyle\frac`的意思

$$
  x_1^*=\dfrac{a_{22}r_1-a_{12}r_2}{a_{11}a_{22}-a_{12}a_{21}}
$$

### 多行方程组对齐
> 注：& 可以用来对齐

$$\begin{cases} 
  a_{11}x_1&+&a_{12}x_2&+&\cdots&+a_{1n}x_n&=&b_1\\
  &&&&\vdots\\
  a_{n1}x_1&+&a_{n2}x_2&+&\cdots&+a_{nn}x_n&=&b_n&			
\end{cases}$$

<!-- ###多行公式等号对齐
$$\begin{eqnarray}f(x,y)
  &=&2xy+(x-y)^2\\
  &=&x^2+y^2
\end{eqnarray}$$ -->

### 大括号右多行赋值
方法1：用 array
$$\left\{\begin{array}{cc} 
		1, & x=f(Pa_{x})\\ 
		0, & other\ values 
	\end{array}\right.$$

方法2：用 cases
$$P(x|Pa_x)=\begin{cases} 
		1, & x=f(Pa_{x})\\ 
		0, & other\ values 
	\end{cases}$$

### 矩阵
或将表示五列的{ccccc}换成表示两行的{ll}
$$\left(\begin{array}{ccccc}
		1 & 2 & 3 & 4 & 5\\ 
		3 & 4 & 5 & 6 & 7
	\end{array}\right)$$

### 求和符号上下限位置
默认：
- 行内公式上下限标注在右侧 $\sum_{k=1}^n{x_k}$
- 行间在上下 $$\sum_{k=1}^n{x_k}$$

可强制修改：
- 行内在上下 $\sum\limits_{k=1}^n{x_k}$
b）行间在右侧 $$\sum\nolimits_{k=1}^n{x_k}$$

### 求和符号下多行限制条件
$$\prod_{k_0,k_1,\ldots>0\atop 
    k_0+k_1+\cdots=n}
  {A_{k_0}A_{k_0}\cdots}$$

### 条件偏导
$$
  \left.\frac{\partial f(x,y)}{\partial x}\right|_{x=0}
$$

### 数学符号字体
- 斜体加粗 A：$\boldsymbol{A}$

### 空白类型列举
两个quad空格 | a \qquad b | $a \qquad b$ | 两个m的宽度
-|-|-|-
quad空格 | a \quad b | $a \quad b$ | 一个m的宽度
大空格 | a\ b | $a\ b$ | 1/3m宽度
中等空格 | a\;b | $a\;b$ | 2/7m宽度
小空格 | a\,b | $a\,b$ | 1/6m宽度
没有空格 | ab | $ab$ | 正常宽度
紧贴 | a\!b | $a\!b$ | 缩进1/6m宽度

