---
title: "modeling a quant matmul in hardware"
date: 2022-11-20T09:03:20-08:00
draft: false
math: true
---
# Introduction
一般在定点硬件上矩阵乘法都是用定点数来乘的，可以设计成一个定点模型
例如
$$q_o = q_i * q_w * 2>>shift$$
> 这里假设硬件只支持Power of 2的scale

这个式子是怎么和浮点乘法联系起来的呢，本篇试图逆向去推出一个浮点和定点乘法的关系,用浮点去模拟这个定点乘法


# 用浮点QDQ模拟定点乘法

$$s_i * s_w * 2>>{shift} = s_o$$

原始的浮点乘法：
$$ r_i * r_w ==> [s_i * q_i] * [s_w * q_w]$$

输出的量化参数在PTQ是我们提前校准好的，这里直接做一次qdq
$$q_o = [s_i * q_i] * [s_w * q_w] / s_o \\  \hat{r_o} = q_o * s_o = [s_i * q_i] * [s_w * q_w] / s_o * s_o$$
这里看出来对输入和输出别添加qdq节点，等价于定点乘法，误差都来自三次quant带来的clip和round误差
其中式2可以看成$$q_o = q_i * q_w * [s_i * s_w/s_o]$$

Visit the [Hugo](https://gohugo.io) website!



