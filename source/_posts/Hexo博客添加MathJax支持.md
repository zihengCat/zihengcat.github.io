---
title: Hexo博客添加MathJax支持
date: 2018-03-24
tags: [Hexo, Web, JavaScript]
---

# 测试

When $a \ne 0$, there are two solutions to $ax^2 + bx + c = 0$ and they are

{% raw %}
$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$
{% endraw %}

{% raw %}
$$
f(x) = \int_{-\infty}^\infty
    \hat f(\xi)\,e^{2 \pi i \xi x}
    \,d\xi
$$
{% endraw %}


{% raw %}
$$
\overrightarrow{AB} + \overrightarrow{BC} = \overrightarrow{AC}
$$

$$
\overrightarrow{v} + \overrightarrow{w} = \overrightarrow{vw}
$$

$$
\overrightarrow{v} = 
\begin{bmatrix}
   v_1  \\
   v_2  \\
   ...  \\
   v_n
\end{bmatrix}
$$

{% endraw %}

## 函数极值

{% raw %}
$f(x)$在区间$[a, b]$上连续可导，
对于$x \in (x-\xi, x+\xi) \mid \xi>0$，
存在$f(\delta) > f(x)$，
则$f(\delta)$ 是$f(x)$在区间$\lbrack a, b \rbrack$上的极大值。
{% endraw %}

## 洛必达法则

{% raw %}
$$
\lim_{x \to c} \frac{u(x)}{v(x)} 
= 
\lim_{x \to c} \frac{u^{\prime}(x)}{v^{\prime}(x)}
$$
{% endraw %}

## 定积分

{% raw %}
$$

\int_{a}^{b}f(x)d{x} = 
      \lim_{n \to \infty}\sum_{i=1}^{n} \Delta x \cdot f(x_i)
$$

$$
\Delta x = \frac{b-a}{n}
$$

$$
x_i = a + \Delta x \cdot i
$$
{% endraw %}

## 微积分基本定理

{% raw %}
$$

\frac{d}{dx}\int_{a}^{x}f(t)dt = f(x)


$$
{% endraw %}

