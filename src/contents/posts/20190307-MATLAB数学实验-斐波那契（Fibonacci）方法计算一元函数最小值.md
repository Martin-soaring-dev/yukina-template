---
title: MATLAB数学实验 - 斐波那契（Fibonacci）方法计算一元函数最小值
published: 2019-03-07
description: ""
tags: [MATLAB, 学习笔记]
category: 学习笔记
draft: false
author: Martin
sourceLink: "https://blog.csdn.net/qq_27387809/article/details/88321914"
---

# Matlab数学实验 - 斐波那契（Fibonacci）方法计算一元函数最小值

[TOC]



# 一、斐波那契（Fibonacci）方法简介

斐波那契（Fibonacci）方法基于斐波那契（Fibonacci）数列，关于fei数斐波那契以百度，在这就不说了。

斐波那契方法，是一种计算最值的方法，其基本思想类似二分法，不断缩小区间，从而实现计算最小值。

其缺点也是显而易见的，类似于二分法，只能求得局部的极值。是一种较为简单的计算方法。

# **二、算法实现步骤**

（后续补上）

# 三、算法代码

斐波那契方法的代码如下：

```matlab
function anwser=method_fibonacci(a,b,e,formula)
%%  斐波那契方法计算最小值(说明与示例)
%   a:          区间左端点
%   b:          区间右端点
%   e:          精确度（最终区间与初始区间的比值，小于1）
%   formula:    对应的公式，格式为文本格式，具体参见例子
%
%   例1：
% method_fibonacci(-1,3,0.32,'x^2-x+2');
%
% 最小值横坐标为：
%    0.5385
% 
% 最小值解为：
%     1.7515
%
%
%   例2：
% method_fibonacci(-1,3,0.01,'x^2-x+2');
% 最小值横坐标为：
%     0.5016
% 
% 最小值解为：
%     1.7500
%%  Step1 初始化
c=(b-a)/e;
n=1;
f=formula;
while fibonacci(n)<c% 确定迭代次数
    n=n+1;
end
% disp(n);
x1=a+fibonacci(n-2)/fibonacci(n)*(b-a);
x2=a+fibonacci(n-1)/fibonacci(n)*(b-a);
f1=strrep(f,'x','x1');
f2=strrep(f,'x','x2');
F1=eval(f1);
F2=eval(f2);
fa=strrep(f,'x','anwser');
%%  Step2 迭代结构
for k=1:n-1
%     X1(k)=x1;
%     X2(k)=x2;
%     F1(k)=F1;
%     F2(k)=F2;
    if F1<F2
        b=x2;
        x2=x1;
        F2=F1;
        x1=a+fibonacci(n-k-2)/fibonacci(n-k)*(b-a);
        F1=eval(f1);
    elseif F1>=F2
        a=x1;
        x1=x2;
        F1=F2;
        x2=a+fibonacci(n-k-1)/fibonacci(n-k)*(b-a);
        F2=eval(f2);
    end
    
end
%% 精确值判断
if F1<F2
    b=x2;
    x2=x1;
    F2=F1;
elseif F1>=F2
    a=x1;
end
x1=x2-0.1*(b-a);
F1=eval(f1);
if F1<F2
    anwser=0.5*(a+x2);
elseif F1==F2
    anwser=0.5*(x1+x2);
elseif F1>F2
    anwser=0.5*(x1+b);
end
disp('最小值横坐标为：')
disp(anwser)
disp('最小值解为：')
disp(eval(fa))
% X1
% X2
% F1
% F2
end
```



斐波那契数列函数代码如下：

```matlab
function f=fibonacci(n)
% 用于产生斐波那契数
n=n+1;
if n>=0
    a=(1+sqrt(5))/2;
    b=(1-sqrt(5))/2;
    c=a.^n-b.^n;
    f=c/sqrt(5);
else
    error('输入有误！请输入正整数(列)');
end
end
```



# 四、样例

```matlab
method_fibonacci(-1,3,0.32,'x^2-x+2');
```

![案例1](https://1drv.ms/i/c/56ceee16a207188d/IQTzqAJQJGOWR7ALCdD2q7KoAbf3GPvstklRsU1lz4Y4Q8c?width=343&height=115)

![绘图1](https://1drv.ms/i/c/56ceee16a207188d/IQQ7rZdVnu74TLE6RUS4QQUGAboSU7QB_q22sJDhHzAgbN8?width=560&height=420)

```matlab
method_fibonacci(-1,3,0.01,'x^2-x+2');
```

![案例2](https://1drv.ms/i/c/56ceee16a207188d/IQSaAxaFnoDqQ7GSCnMQ41zSASyZf-9--6hM3R4bEeEaK10?width=344&height=115)

![绘图2](https://1drv.ms/i/c/56ceee16a207188d/IQQx9cPFU0V9RaB8fGlAqgjKAbAtB0Pxtbdj2io1cdvLtwc?width=560&height=420)

