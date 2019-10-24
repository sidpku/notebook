---
title: Matlab学习笔记
date: 2019-05-30 15:20:27
update: 2019-05-30 15:20:27
mathjax: true
tag:
- Matlab
categories:
- note
---

# matlab学习笔记

2019.7.16更新

发现matlab在地理、气象数据分析上有很多知识需要积累，都记在这里啦。

---

进课题组就要会用Matlab!我来了！

<!-- more -->

## 变量

### 定义变量
```
a=1
```
> MATLAB中所有的变量都是向量
> 矩阵是一个通常用在线性运算中的二维向量

#### 特殊变量

|变量|代码|
|---|---|
|$e$|`exp(1)`|
|$\pi$|`pi`|

### 定义矩阵
#### 创建行向量
``` 
a=[1,2,3,4] 或者 a=[1 2 3 4]
```
#### 创建多行矩阵
```
a=[1 2 3;4 5 6; 7 8 9] 注意 ; 后面的空格
```
>虚数单位用`i` 或者`j`表示


#### 使用函数创建矩阵

a=ones(3,1)

a= 3 &times; 1
&emsp;&emsp;1
&emsp;&emsp;1
&emsp;&emsp;1

类似的函数有`ones`、`rand`、`zeros`。

### 定义结构体

```matlab
s.name='alice'
s.score=90
```

不需要声明类型和结构，直接赋值即可创建。

可直接按照下标赋值，中间跳过的元素会自动赋值为空(`[]`)。

赋值方法有两种，一种是上面的直接赋值，另一种是通过语法赋值:

```matlab
s(index) = struct('tag1',[value1],'tag2',value2,...)	
# 注意tag的数量和名称必须和已经定义的s一致

[EX]
s(i)=struct('alice')
```

直接赋值可以改变结构体结构，但使用struct赋值必须遵守原来的结构。如果某一值为空，可如下写：

```matlab
s(i) = struct('name','alice','score',[])
```

> 尽量不要为空，感觉matlab中很多函数对于空的支持不够好。
>
> null在matlab中是矩阵的零空间。

#### 结构体中元胞数组的应用

使用元胞数组，可以实现结构变量的列表化使用。什么意思呢？看例子吧。


```matlab
> struct('name',{'alice','bob','carol'},'age',{1,2,3})	%定义变量
% 此时，struct的大小是1*3，和元胞数组的大小相同。
```
**引用**

```matlab
> struct(1,2)
ans=
	name:'bob'
	age:2
>struct(1,3)
ans= 
	name:'carol'
	age:3
% 返回的是一个struct。
> tmp=struct(1,2)
ans=
	包含以下字段的struct:
	name:'bob'
	age:'2'
```


### 数组的索引

制定行和列下标

```matlab
a ( 4 , 2)
ans =
		14
```

线性索引，按照列进行遍历，得到的单数字索引

```matlab
a(8)
ans =
		14
```

**注意**：索引如果出边界会提醒错误

> 如果( i, j )在边界外，则 a( i, j) = x (x为给定值)可以进行赋值，其他位置自动补为0

```matlab
a(8,4:1:10)
a(8,:)
a(8,1:3)
a(x,start:step:end)
```

### 字符串

建立一个数组存放字符串，通过索引可以得到不同的字符串。

```matlab
name = [string('alice'),string('bob')]
name =
	1 * 2 string 数组
	"alice" "bob"

name(1)
ans = 
	string
	"alice"
```

### 文本和符号

严格来讲，文本其实是储存了字符的数组，下标索引到的是字符。

```
mytext='hello, world! '
mytext='you''re my lover'  %如果语句中需要有单引号，则输入两个单引号进行转义
```

```matlab
f=71
c=(f-32)/1.8
mytext=['the temperature is ',num2str(c),'C']
```

输出为：

mytext = 

'the temperature is 21.6667C'

### 数据类型转换

数字数组:arrow_right:字符串:`string(array)`

### 工作区变量

```
whos #查看工作区变量
clear #清空工作区变量
save #保存工作区变量为mat文件
load xxx.mat #加载变量到工作区
```

#### save 函数

```matlab
% 将 变量1，变量2 保存在 <filename>.mat 文件中。
save('filename','Varable1','Variable2'...);

% 将变量保存成txt文件
> save('<filename>.txt','variable1','variable2',...,'-ascii');
```

## 运算

### 矩阵和向量运算

1. `a+10`即 $a_{i,j}=a_{i,j}+10)$
2. `sin(a)`即$ans_{i,j}=sin(a_{i,j})$
3. `a'`即转置
4. `a*a`即矩阵乘法
5. `inv(a)`或`a^-1`进行求逆
> `a.*a`即$ans_{i,j}=a_{i,j}\times a_{i,j}$

#### 串联（concatenation）

A=[a,a]
A= 3 $\times$ 6
    1 2 3 1 2 3
    4 5 6 4 5 6
    7 8 9 7 8 9

## 高级功能

### 函数

```matlab
a=[1,2,3]
max(a)	%将会返回 ans = 3
b=[2,3,4]
max(a,b) %多个参数用逗号分隔
[max_num,location]=max(a,b)	%返回值有多个参数
disp('hello')	%将字符用单引号括起来
```

常用函数

| 函数      | 功能               |
| :-------- | :----------------- |
| clc       | 清屏               |
| close all | 关掉所有的绘图窗口 |
| clf       | 清楚绘制的图形     |

#### 产生矩阵函数

:point_right: 产生随机数

产生均匀分布的假随机数（uniformly distributed pseudorandom numbers）

**rand(m,n,...,z)**

产生一个$m\times n\times \cdots \times z$的矩阵，元素为0-1均匀分布的随机数。

**randi(max,m,n,...,z)**

产生一个$m\times n\times \cdots \times z$的矩阵，元素为1-max均匀分布的随机整数。

`randi([min,max],m,n)`可以产生一个$m*n$的菊展，元素为min-max的随机整数。

**randn(m,n,...,z)**

产生一个$m\times n\times \cdots \times z$的矩阵，元素标准正态分布的随机数。

#### 矩阵变换函数：

:point_right: fliplr函数

将矩阵的第二维调换顺序。

[EX] 对应的数学变化是：
$$
X=
\left[
\begin{matrix}
1&2&3\\
4&5&6
\end{matrix}
\right]
\rightarrow
X=
\left[
\begin{matrix}
3&2&1\\
6&5&4
\end{matrix}
\right]
$$


#### 元素处理函数

| 函数名 | 功能 |
| ------ | ---- |
| `ceil`     |向上取整      |
|`floor`|向下取整|
|`round`|向周围取整(0.5取距离原点最远的)|
#### 索引函数

:point_right: **find**

找到矩阵中的非零元素的索引

:point_right:  **isnan**

找到矩阵中的`NaN`值的坐标索引，若为`NaN`返回`1`，否则返回`0`。

> 将`find`函数和`isnan`函数结合起来，可以得到判断向量中是否有`NaN`的组合函数。
>
> e.g. `if find(isnan(X))`，若`X`中含有`NaN`,就会执行if判断句后面的语句。

#### 时间函数

:point_right:  **datenum**函数

返回一个绝对日期（相对于**0000年1月0日**），可用于计算过去了多少天。

```matlab
% [EX]一个计算DOY的小程序。
day_of_year = datenum(year,month,day_of_month)-datenum(year,01,01)+1
```
:point_right: **datestr** 函数
返回以字符串表示的时间。

```matlab
> String_of_date = datestr(now,format_id) % 返回现在的时间
```
|format_id|样式|
|:---:|:---:|
|30|'20010812T030340'|
|31|'2009-12-20 12:23:10'|

:point_right: **datetime数据类型**

**获取分量**

```matlab
date = datetime(1999,1,1,12,0,0);
% get value
day = date.Day ;
year = date.Year;
% assignment
date.Year = 2018
% etc.
```




#### 数学方法

##### 最小二乘法

:point_right: **polyfit**函数

```matlab
> P = polyfit(x,y,n);
% x是自变量
% y是因变量
% n是多项式拟合的最高次数
```

>  查看doc文档，看和假设检验的联动。

#### 地学方法

##### 三角网插值

```matlab
value = griddata(lat,lon,data,obj_lat,obj_lon);
% lat,lon,data要保持矩阵大小一致。
% obj_lat,obg_lon与value的大小保持一致。
```

首先进行Delaunay三角剖分，然后默认使用linear方法进行插值。

> 修改method可以通过不同的方式进行插值。`value=griddata(____,method); `

更多内容参考：:globe_with_meridians: [内插散点数据,MathWorks](https://ww2.mathworks.cn/help/matlab/math/interpolating-scattered-data.html#bsovi2t)

### 二维图、三维图

#### 绘制二维图：
```matlab
x=0:pi/100:2*pi;
y=sin(x);
plot(x,y); %绘制二维图
```
#### 绘制三维图：
```matlab
[x,y]=meshgrid(-2,.2,2); %通过meshgrid函数生成一组点
z=x.*exp(-x.^2-y.^2);
surf(x,y,z)	%绘制面和线的彩图
mesh(x,y,z)	%绘制线的彩图
```
绘图函数surf和mesh的效果图：
![效果图](/note-for-matlab/mesh和surf函数的区别.jpg)

#### 绘图控件函数：
```matlab
x=0:pi/100:2*pi;
y=sin(x);
plot(x,y);
hold on;
z=cos(x);
plot(x,z);
legend('sin','cos');	%能够按照顺序加标签。
hold off;	%记得取消锁定啊

% 绘制左右坐标
yyaxis left
plot(x,y);
ylabel('left side');

yyaxis right
plot(x,y);
ylabel('right side');
```
hold的效果：
![xxx](/note-for-matlab/绘图控件函数hold的用法.jpg)

```matlab
[x,y]=meshgrid(-2,.2,2);
z=x.*exp(-x.^2-y.^2);
subplot(1,3,1);mesh(x);title('x');
subplot(1,3,2);mesh(y);title('y');
subplot(1,3,3);mesh(x,y,z);title('z');
```
subplot函数的效果图：
![xxx](/note-for-matlab/绘图函数控件-subplot函数的效果图.jpg)

#### pcolor函数

```matlab
> pcolor(col,row,flip(Matrix,1)) % pcolor函数坐标原点为原矩阵的（1,1）
```

#### shading函数

```matlab
> shading flat; % 平面着色，去掉网格线的黑色。
```

#### axis函数

```matlab
> axis on % 控制坐标轴步长相同
> axis equal
> xtickangle(45)	% 将坐标轴标签正方向旋转45度。
> xticks([])		% 删除x轴刻度线
```



#### 绘图函数总结：

| 函数名称       | 函数功能               |
| :------------- | :--------------------- |
| plot(x,y)      | 绘制线图               |
| mesh(x,y,z)    | 绘制三维线图           |
| surf(x,y,z)    | 绘制三维面图           |
| hold on(off)   | 在一张图中在一起的图像 |
| subplot(x,y,z) | 一张图中多个分开的图像 |

### 脚本和编程
```matlab
edit name	%创建名字为name的脚本
```
保存： CTRL+S
运行：
      1. F5
      2. 键入脚本名称

#### 循环

```matlab
nsamples = 5;
npoints = 50;

for k = 1:nsamples
    iterationString = ['Iteration #',int2str(k)];
    disp(iterationString)
    currentData = rand(npoints,1);
    sampleMean(k) = mean(currentData);
end
overallMean = mean(sampleMean)
```
#### 判断

```matlab
if overallMean < .49
   disp('Mean is less than expected')
elseif overallMean > .51
   disp('Mean is greater than expected')
else
   disp('Mean is within the expected range')
end
```
脚本位置：
希望正常运行脚本，需要位于当前文件夹或者搜索路径中的某个位置。
如果要将程序存储在其他文件夹，或者运行其他文件夹中的程序，请将其添加到搜索路径中。

> 添加搜索路径:文件夹浏览器选中相应的文件夹，右键选择添加到路径。

#### 自定义函数

```matlab
function [output1,output2,...] = <function_name>(input1,input2,...)

% 注释部分，能够通过help调用。

code1;
code2;

end
```

#### 断点

在编辑器中使用`F12`添加断点。

在编辑器界面按`F5`运行代码，至断点行的上一行中止。

#### 中止脚本

```matlab
if <expression>
	break;	%满足条件中止脚本
end	
```

#### 错误信息处理
如果`try`部分语句报错，则直接进入`catch`部分的语句处理。
```matlab
try
	expression;
catch
	expression;
end
```



### 帮助文档

```matlab
help mean %打开mean的简单帮助文档
doc mean %在新窗口中打开mean的帮助文档
```

### 矩阵和幻方矩阵

```matlab
sum(A)	%对矩阵A求和
sum(A,1) %对矩阵A列向量求和，结果为一个行向量
sum(A,2)	%对矩阵A行向量求和，结果为一个列向量
A'	%对矩阵A进行转置，如果为复数，则改变虚部的符号（通过.'来避免改变虚部）
```

### 其他语法

```matlab
> s=1+2+3+4+5...	%通过三个句号实现换行。
+5+6+7+8;

> pause(n)	% 暂停n秒
```


## 文件

### 文件目录

```matlab
dir()	%返回目录中的文件列表
delete('<filepath&filename>')； % 删除文件
rmdir('dir_path_name');	% 删除文件夹
```

**注意：返回的列表中包含`·`&`··`这两个目录名**，在使用遍历的时候要记得去掉。

### .xlsx文件

* 可以直接在matlab中打开，通过图形界面读取数据
* 命令行

```matlab
data = xlsread('<filename.xlsx>',SheetIndex,'A1:G2');
%	返回array
%	SheetIndex: int
%	'A1:G2':	char vector,读取变量的范围
```

### .txt文件

将数据写到文件中

```matlab
 FileId = fopen('<filename.txt>','w+');
 For i  =  1 : length(Output_List)
 	fprintf('%s\n',transpose(Output_List(i,:));
 end
```

从文件中读数据

```matlab
FileID = fopen('test.txt','r');
tline = fgetl(FileID);
count = 1;
while ischar(tline)
	content(count,:) = tline; 
	tline = fgetl(FileID)
	count = count + 1;
end
fclose(FileID);
```

:exclamation: 下面这种写法是不行的,会导致全部以`ASCII`码表示。

```matlab
FileID = fopen('test.txt','r');
tline = fgetl(FileID);
content = [];
while ischar(tline)
	content(end + 1,:) = tline; 
	tline = fgetl(FileID)
end
fclose(FileID);
```

### .csv文件

如何将数据写入`.csv`文件？下面是代码。

```matlab
Filename = ['something',datestr(now,30),'.csv'];	
% 文件名，datestr()可以返回字符串表示的时间。
% 加.csv的后缀的话，save（filename）就不会自动加.mat的后缀
FileID = fopen(Filename,'w');	% 打开文件
[line_range,~] = size(Obj.pre);	% 获取 数据行数

% 打印行首,行首不同列之间要以`,`分隔。
headline = 'something';
fprintf(FileID,'%s\r\n',headline);

% 处理NaN数据。原数组Obj.value是double类型的矩阵，
% 转字符串后NaN会变成<missing>,在输出和join的时候会出错，所以替换成 '' (两个单引号，中间无空格)
Obj.pre =  string(Obj.pre);
missing_index = find(ismissing(Obj.pre));
Obj.pre(missing_index) = '';

% 按行打印数据
for i = 1: line_range
    year = i - 1 +1950;
    line_content = join(Obj.pre(i,:),',');
    fprintf(FileID,'%d,%s\r\n',year,line_content);	% 设置好每行的格式
end

fclose(FileID);   % 关闭文件
```

### .tif文件

```matlab
% 读取文件中的信息，读到的Variable是一个结构体。
[Variable,info]=geotiffread('filename')
```

### .nc4文件

```matlab
% 读取文件的基本信息，快速获取数据结构。
>> ncdisp('filename')

% 读取文件中的某一变量。
>> Var=ncread('filename','Variables_Name');
```

:star: 这儿有一个不错的例子（or教程）

[How to read and plot MERRA-2 NetCDF data in MATLAB](https://disc.gsfc.nasa.gov/information/howto?title=How%20to%20read%20and%20plot%20MERRA-2%20NetCDF%20data%20in%20MATLABc)

> The instructions show how to extract and plot data from NetCDF files using MATLAB

代码在这里

```matlab
% read-in variables
var1 = ncread(file, 'TO3');
% MATLAB data is oriented by (Y,X), but the data was written as (X,Y).
% The rot90 and fliplr function correctly orient the data.
var1 = rot90(fliplr(var1));
lats = ncread(file, 'lat');
lons = ncread(file, 'lon');
% ========== Plot Data ========================
pcolor(lons,lats,var1(:,:,1));
shading flat	% 突然拥有颜色
c = colorbar;	% 加入彩条
ylabel(c,'Dobsons')
load coast
hold on
plot(long,lat,'black')
title('MERRA-2 Total Column Ozone')
xlabel('degrees longitude')
ylabel('degrees latitude')
```



## 环境配置

### 搜索路径

![如何配置搜索路径](/note-for-matlab/搜索路径的配置.png)

将常用的数据文件夹加入到搜索路径中，下次调用数据就不需要写全局路径了。

### Toolbox 安装

1. 将Toolbox放置到目标文件夹

   路径：`E:\program\matlab\Toolbox\`

2. 添加路径

   `主页`:arrow_right:`设置路径`

   将所选的Toolbox的文件夹加入到路径中去。

3. 更新Toolbox路径

   `主页`:arrow_right:`预设`:arrow_right:`常规` 点击`更新工具箱路径缓存`



## Command Line Mode

命令行下常见命令

|命令|作用|
|---|---|
|`cd`|查看当前路径|
|`who`|查看工作区变量|

