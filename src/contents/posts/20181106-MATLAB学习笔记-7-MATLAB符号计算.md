---
title: MATLAB学习笔记——7 MATLAB符号计算
published: 2018-11-06
description: ""
tags: [MATLAB,学习笔记]
category: 学习笔记
draft: false
author: Martin
sourceLink: "https://blog.csdn.net/qq_27387809/article/details/83759489"
---

# MATLAB学习笔记——7 MATLAB符号计算
一下是.m文件内笔记的内容

```matlab
%% 7.1 符号对象
%% 1. 符号对象的建立
%(1)sym函数 调用格式：符号对象名=sym(A)
%例7.1.1
t=sym(2);
t+1/2
pause(2)
sin(sym(pi/3))
pause(2)
sin(pi/3)
pause(2)
%%
a=5;
b=-8;
x=sym('a');
y=sym('b');
w=(a+b)*(a-b)
pause(2)
s=(x+y)*(x-y)
pause(2)
eval(s)
pause(2)
% 符号运算的结果：精确的表达式
% 数值运算的结果：一个数值
% syms命令 格式：syms a b c d...
%% 2.符号对象的运算
%% (1)四则运算 + - * / ^
%例7.1.2
syms x;
f=2*x^2+3*x-5;
g=x^2-x+7;
f+g
%% (2)关系运算	<    <=  >   >=   ==  ~= 
%      分别对应   lt() le() gt() ge() eq() ne()
%assume() 设置符号对象的值域
%格式： assume(condition) assume(expr,set)
%例7.1.3
syms x;
assume(x<0);
abs(x)==x
%例7.1.3
assume(x,'positive');
abs(x)==x
%% (3)逻辑运算 & | ~
%and(a,b) or(a,b) not(a) xor(a,b)
%例7.1.4
syms x;
y=x>0&x<10
pause(2)
y=and(x>0,x<10)
%% (4)因式分解与展开运算
% ①factor(s)：对符号表达式s分解因式
% ②expand(s)：对符号表达式s进行展开
% ③collect(s)：对符号表达式s合并同类项
% ④collect(s,v)：对符号表达式s按变量v合并同类项
%例7.1.5
syms a b;
s=s^3-b^3;
factor(s)
pause(2)
factor(12)
pause(2)
%% 梅森素数的验证问题。
%例7.1.6：请验证M19 、 M23 、 M29 、 M31是否为梅森素数。
%Mp=2^p-1   (p为素数，且Mp为素数)
syms p;
m=2^p-1;
p=19;
m19=eval(m)
factor(m19)
p=23;
m23=eval(m)
factor(m23)
p=29;
m29=eval(m)
factor(m29)
p=31;
m31=eval(m)
factor(m31)
%% (5)其他运算
% ①提取有理分式的分子分母：[n,d]=numden(s)
% ②提取符号表达式的系数：c=coeffs(s,x)
% ③符号表达式简化：simplify(s)
% ④符号多项式与多项式系数向量之间的转换：
%   符号多项式转化为多项式系数向量：p=sym2poly(s)
%   多项式系数向量转化为符号多项式：s=poly2sym(p)
%例7.1.7 求方程ax^2+bx+c=0的根。
syms a b c x;
f=a*x^2+b*x+c;
g=coeffs(f,x);
g=g(end:-1:1);
disp(roots(g))
%% 思考：为何不能用p=sym2poly(s)？
syms a b c x;
f=a*x^2+b*x+c;
g=sym2poly(f)
%接着会显示报错信息：
%错误使用 sym/sym2poly (line 21)
%Input has more than one symbolic variable.
%% (6)符号运算中变量的确定
% ①若未明确指明自变量，MATLAB将按照以下原则确定主变量并对其进行相应运算：
%   寻找初i、j之外，在字母上最接近x的小写字母；
%   若表达式中有两个符号变量与x距离相等，则ASCII码大者优先。
% ②symvar()函数可以用于查找一个符号表达式中的符号变量，函数的调用格式为：
%   symvar(s,n)
%   函数返回符号表达式s中的n个符号变量。因此用symvar(s,1)查找表达式s的主变量。
%% 3.符号矩阵
%例7.1.8
syms a b x y alp;
m=[a^3-b^3,sin(alp)^2;(15*x*y-3*x^2)/(x-5*y),78];
disp 'm='
disp(m)
ans1=simplify(m);
disp 'simplified m :'
disp(ans1)
%% 
%例7.1.9 当λ取何值时，以下齐次线性方程组有非零解。
syms lamda;
A=[1-lamda,-2,4;2,3-lamda,1;1,1,1-lamda];
D=det(A);
factor(D)
%% 练习
%% 1、分解因式
%% (1)
syms x;
f=factor(x^9-1);
disp(f)
%% (2)
syms x;
f=factor(x^4+x^3+2*x^2+x+1);
disp(f)
%% 2、GIMPS
% 导航：https://www.equn.com/forum/thread-9246-1-1.html
% 官网：https://www.mersenne.org/
%%
%% 7.2 符号微积分
%% 1、符号函数的极限
% limit(f,x,a)
% 求函数f关于变量x在a点的极限
% 求单边极限
% limit(f,x,a,'right')
% limit(f,x,a,'left')
% 例 7.2.1 求下列极限。
syms a m x n;
f=(x^(1/m)-a^(1/m))/(x-a);
limit(f,x,a)
g=(1+1/n)^n;
limit(g,n,inf)
%% 2、符号函数的导数
% diff(f,x,n)
% 例 7.2.2 求下列函数的导数。
syms  x y z;
f=sqrt(1+exp(x));
diff(f)                 
g=x*exp(y)/y^2;
diff(g,x)
diff(g,y) 
%% 3、符号函数的积分
%% (1)不定积分
% int(f,x)
% 求函数f对x的不定积分
% 例 7.2.3 求下列不定积分。
syms x t;
f=(3-x^2)^3;
int(f)
g=5*x*t/(1+x^2);
int(g,t)      
%% (2)定积分
% int(f,x,a,b)
% 例 7.2.4 求下列定积分。
syms x t;
int(abs(1-x),1,2)
int(1/(1+x^2),-inf,inf)
int(4*x/t,t,2,sin(x))
%% 河道水流量的估算问题。
xi=0:50:600;
yi=[4.4,4.5,4.6,4.8,4.9,5.1,5.4,5.2,5.5,5.2,4.9,4.8,4.7];
p=polyfit(xi,yi,3);
plot(xi,yi,'o',xi,polyval(p,xi));
syms y x;
y=poly2sym(p,x);
s=int(y,x,0,600);     
v=s*0.6;             
eval(v)
yn=-yi;
p=polyfit(xi,yn,3);
plot(xi,yn,'o',xi,polyval(p,xi));
syms y x yii;
y=poly2sym(p,x);
yii=diff(y,x); 
x=50:60;                                    
k=eval(yii);                                
all(abs(k)<1/1.5)
%% 练习
%% 1. 求函数的极限。
syms x;
f1=[1 -6 8];
f2=[1 -5 4];
f1=poly2sym(f1,x);
f2=poly2sym(f2,x);
g=f1/f2;
ans1=limit(g,x,4);
disp 'answer for (1) is:'
disp(ans1);
pause(1)
ans2=limit(abs(x)/x,x,0,'left');
disp 'answer for (2) is:'
disp(ans2);
%% 2. 求函数的符号导数。
syms x;
y1=sin(x)-x^2/2;
ans11=diff(y1,x,1);
ans12=diff(y1,x,2);
disp '（1）计算结果：'
disp 'y''='
disp(ans11)
disp 'y''''='
disp(ans12)
pause(1)
clear x y
syms x y
z=x+y-sqrt(x^2+y^2);
ans21=diff(z,x,1);
ans21=diff(ans21,y,1);
ans22=diff(y,x,1);
disp '（2）计算结果：'
disp 'd^2z/dxdy='
disp(ans11)
disp 'dy/dx='
disp(ans12)
%% 3.用符号计算求给定函数的定积分。
syms x y
y1=x*(2-x^2)^12;
y2=abs(log(x));
Y1=int(y1,x,0,1);
Y2=int(y2,x,exp(-1),exp(1));
Y2=eval(Y2);
disp 'Y1='
disp(Y1)
disp 'Y2='
disp(Y2)
%%
%% 7.3 级数
%% 1、级数求和
% symsum(s,v,n,m)
% s：级数通项
% v：求和变量
% n：求和初值
% m：求和末值
% 例 7.3.1 求下列级数之和。
syms n;
s1=symsum(n^2,1,100);
s2=symsum((-1)^(n-1)/n,1,inf);
s3=symsum((-1)^(n-1)/(2*n-1),n,1,inf);
hypergeom([-1/2, 1], 1/2, -1);     %超几何函数
%eval(s3)*4;
disp 's1='
disp(s1)
disp 's2='
disp(s2)
%disp 's3='
%disp(s31)
%第三个级数求解有问题
%% 银行利率的计算问题。
syms k r;
p2=symsum(50000*(1+0.045/k)^k,k,2,2);
eval(p2)
p4=symsum(50000*(1+0.045/k)^k,k,4,4);
eval(p4)
p12=symsum(50000*(1+0.045/k)^k,k,12,12);
eval(p12)
limit((1+r/k)^k,k,inf)
50000*exp(0.045)
%% 2、泰勒级数
% taylor(f,v,a,Name,Value)
% 将函数f按变量v在a点展开为泰勒级数，a默认值是0，
% Name和Value为选项设置，经常成对使用，前者为选项名，后者为该选项的值。
% Name有三个可选取的字符串：
%   ①'ExpansionPoint'：指定展开点，对应值可以是标量或向量。未设置时，展开点为0.
%   ②'Order'：指定截断参数，对应值为一个正整数。未设置时。截断参数为6，即展开式的最高阶为5。
%   ③'OrderMode'：指定展开式采用绝对阶或相对阶，对应值为'Absolute'或'Relative'。默认'Absolute'
% 例 7.3.2 求函数f(x)在x=1处的5阶泰勒级数展开式。
syms x;
f=(1+x+x^2)/(1-x+x^2);
taylor(f,x,1,'Order',6)
expand(ans)
%% 例 7.3.3 利用泰勒展开式计算三角函数的值。
syms x;
f=taylor(cos(x),x,pi)
x=3;
eval(f)
cos(3)
%% 练习
%% 1.求下列级数之和。
syms xi n x
xi=(-1)^(n+1)*(2*n-1)/(2^(n-1));
S1=symsum(xi,n,1,inf);
xi=x^(2*n-1)/(2*n-1);
S2=symsum(xi,n,1,inf);
disp 'S1='
disp(S1)
disp 'S2='
disp(eval(S2))
%% 2.求函数在x=x0的泰勒级数展开式。
syms x 
y1=(exp(x)+exp(-x))/2;
S1=taylor(y1,x,0,'Order',5);
y2=tan(x);
S2=taylor(y2,x,2,'Order',3);
disp 'S1='
disp(S1)
disp 'S2='
disp(S2)
%% 7.4 符号方程求解
%% 1、代数方程符号求解
% solve()
%   ①solve(s)：求解符号表达式s的代数方程，求解变量为默认变量
%   ②solve(s,v)：求解符号表达式s的代数方程，求解变量为v
%   ③solve(s1,s2,...,sn,v1,v2,...,vn)：求解符号表达式s1,s2...sn组成的代数方程组，求解变量分别为v1,v2,...,vn。
% 例 7.4.1 解方程ax^2+bx+c=0。       
syms x y a b c; 
solve(a*x^2+b*x+c==0)
f=a*x^2+b*x+c==0;
solve(f)
solve(a*x^2+b*x+c)
f=a*x^2+b*x+c
solve(f)
%注意：solve()函数求解方程时，所得到的结果不一定准确。
%% 2、常微分方程符号求解
% D表示导数，D3y(6)=y'''(6)
%dsolve(e,c,v)
%用于求常微分方程e，在初值条件c，下的特解。v是自变量，未给出c，则求通解。
%dsolve(e1,e2,...,en,c1,c2,...,cn,v1,v2,..,vn)
% 例 7.4.1 例2 求下列微分方程或方程组的解。    
syms x y t;
y=dsolve('Dy-(x^2+y^2)/x^2/2',x)
[x,y]=dsolve('Dx=4*x-2*y','Dy=2*x-y',t)
%% 疾病传染问题。
syms a b c y t;
f=dsolve('Dy=a*y*(1-y)-c*y', 'y(0)=b',t)
f=dsolve('Dy=a*y*(1-y)-a*y', 'y(0)=b',t)
%% 练习
%% 1.求非线性方程组的符号解。
syms x y
[u,v]=solve([log(x/y)==9,exp(x+y)==3],[x,y])
pause(1)
clear x y z
syms x y z
[a b c]=solve(y==4*x^2/(4*x^2+1),z==4*y^2/(4*y^2+1),x==4*z^2/(4*z^2+1))
%% 2.求微分方程初值问题的符号解，并与数值解进行比较。
clear
clc
syms x y;
dsolve('x^2*D2y-x*Dy+y==x*log(x)','y(1)==Dy(1)==1',x)
```
转载请注明出处