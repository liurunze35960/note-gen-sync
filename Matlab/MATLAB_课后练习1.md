# 大学数学实验第1单元课后练习
## 1.根据你对所学数学软件的学习和了解，请用简洁的语句说明软件中常用的各个命令对应的方法和语法结构，填于表中相应的位置。要求：提供15个命令。
|           |             |                            |     |
| --------- | ----------- | -------------------------- | --- |
| **命 令**   | **对应数学问题**  | **语法结构**                   |     |
| `limit`   | 函数极限        | `limit(f, x, x0)`          |     |
| `diff`    | 函数微分 (求导)   | `diff(f, x, n)`            |     |
| `int`     | 函数积分 (不定/定) | `int(f, x, a, b)`          |     |
| `solve`   | 求解代数方程      | `solve(eqn, var)`          |     |
| `dsolve`  | 求解微分方程      | `dsolve(eqn, cond)`        |     |
| `plot`    | 绘制二维曲线      | `plot(x, y, 'LineSpec')`   |     |
| `surf`    | 绘制三维曲面      | `surf(X, Y, Z)`            |     |
| `syms`    | 定义符号变量      | `syms x y z`               |     |
| `inv`     | 矩阵求逆        | `inv(A)`                   |     |
| `det`     | 计算行列式       | `det(A)`                   |     |
| `eig`     | 求解特征值和特征向量  | `[V, D] = eig(A)`          |     |
| `roots`   | 求解多项式根      | `roots([c1, c2, ..., cn])` |     |
| `polyfit` | 多项式拟合       | `p = polyfit(x, y, n)`     |     |
| `sum`     | 数组元素求和      | `sum(A)`                   |     |
| `mean`    | 计算平均值       | `mean(A)`                  |     |

## 2.找出十个软件中的关键菜单（APP）或功能模块，填入下表。

|        |                                           |
| ------ | ----------------------------------------- |
| **名称** | **菜单或功能模块**                               |
| 1      | 曲线拟合 (Curve Fitting)                      |
| 2      | 优化 (Optimization)                         |
| 3      | 符号数学工具箱 (Symbolic Math Toolbox)           |
| 4      | 信号分析器 (Signal Analyzer)                   |
| 5      | 图像处理 (Image Processing)                   |
| 6      | 统计与机器学习 (Statistics and Machine Learning) |
| 7      | 控制系统设计器 (Control System Designer)         |
| 8      | Simulink                                  |
| 9      | App 设计工具 (App Designer)                   |
| 10     | 深度学习 (Deep Learning)                      |

## 3 .
（1） 试根据help clear的提示，并先用who看现在工作区的变量,然后记录要的变量,再clear掉不要的,最后把记录要的变量也clear。显示代码输入过程。

（2）试根据help plot的提示，归纳plot几种运用格式。

**(1) `clear` 命令操作过程**

`clear` 命令用于从工作区中删除变量，释放内存。

```matlab
a = 10;
b = 'hello';
c = [1, 2, 3; 4, 5, 6];
who
clear a c
who
clear
who
```

**(2) `plot` 命令的几种常用格式**

`plot` 是MATLAB中最核心的绘图函数，用于创建二维线图。

1. **绘制单个向量**:
    
    - `plot(Y)`
        
    - 如果 `Y` 是一个向量，该命令会以 `Y` 的元素值为纵坐标，以元素的索引（1, 2, 3, ...）为横坐标进行绘图。如果 `Y` 是一个矩阵，则会对 `Y` 的每一列分别绘制曲线。
        
2. **指定 X 和 Y 坐标**:
    
    - `plot(X, Y)`
        
    - 这是最常用的格式。`X` 和 `Y` 必须是相同大小的向量或矩阵。它会绘制 `Y` 中每个点相对于 `X` 中对应点的曲线。
        
3. **指定线型、标记和颜色**:
    
    - `plot(X, Y, LineSpec)`
        
    - `LineSpec` 是一个字符串，用于定义曲线的样式。例如，`'-or'` 表示使用红色的 (r)、带圆圈标记的 (o)、实线 (-) 连接的曲线。
        
4. **在一张图上绘制多条曲线**:
    
    - `plot(X1, Y1, LineSpec1, X2, Y2, LineSpec2, ...)`
        
    - 可以在一个 `plot` 命令中包含多组 X, Y 和线型参数，以在同一坐标轴下绘制多条曲线。
        
5. **使用名称-值对参数进行高级定制**:
    
    - `plot(X, Y, 'PropertyName', PropertyValue, ...)`
        
    - 这提供了最灵活的定制方式，可以精确控制曲线的各种属性，如线宽 (`'LineWidth'`)、标记大小 (`'MarkerSize'`)、标记填充色 (`'MarkerFaceColor'`) 等。
        
    - 例如: `plot(x, y, '-s', 'LineWidth', 2, 'MarkerSize', 10)`
        

## 4 .你最感兴趣的数学/计算/数据分析软件功能是什么？

我最感兴趣的MATLAB功能是 **Simulink**。它是一个图形化的仿真环境，允许用户通过拖放模块和连接线来搭建动态系统的模型。与纯代码编程相比，Simulink 的**可视化建模方式**非常直观，能够清晰地展现信号的流向和系统各部分之间的相互作用，特别适合于控制系统、信号处理和物理系统的建模与仿真。这种“所见即所得”的建模方法，不仅降低了复杂系统建模的门槛，也使得模型的调试、验证和分析过程变得更加高效和富有洞察力。

## 5 .第一单元课件上的例1-1至1-4的实现（只用附上可以正确实现的代码，所用语言不限）。

**1-1: 用简洁命令计算并绘制在0≤x≤6范围内的sin(2x)、sin(x²)、(sin(x))²。**

```matlab
% 1. 创建x向量，范围从0到6
x = linspace(0, 6);

% 2. 计算三个函数对应的y值
y1 = sin(2*x);
y2 = sin(x.^2);   
y3 = (sin(x)).^2; 

% 3. 绘图
plot(x, y1, 'g'); % y1用绿色线绘制
hold on;          
plot(x, y2, 'r'); % y2用红色线绘制
plot(x, y3, 'b'); % y3用蓝色线绘制
hold off;     

% 4. 添加图例
title('函数图像: sin(2x), sin(x^2), (sin(x))^2');
xlabel('x');
ylabel('y');
legend('sin(2x)', 'sin(x^2)', '(sin(x))^2');
grid on; % 添加网格
```

**1-2: 求方程 3x⁴+7x³+9x²-23=0 的全部根。**

```matlab
% 方程系数为 3, 7, 9, 0, -23 
p = [3, 7, 9, 0, -23];

% 使用 roots 函数求解多项式的根
x = roots(p);

disp('方程 3x^4+7x^3+9x^2-23=0 的全部根为:');
disp(x);
```

**1-3: 求积分** $\int_{0}^{1}x ln(1+x) dx$

```matlab
% 1. 定义符号变量x
syms x;

% 2. 定义被积函数
f = x * log(1+x); % 在MATLAB中，log() 就是自然对数ln()

% 3. 使用 int 函数计算定积分
result = int(f, x, 0, 1);

disp('积分符号解为:');
disp(result);
```

**1-4: 求解线性方程组**$$\begin{cases} 2x_{1} - 3x_{2} + x_{3} = 4 \\ 8x_{1} + 3x_{2} + 2x_{3} = 2 \\ 45x_{1} + x_{2} - 9x_{3} = 17 \end{cases}$$

```matlab
% 1. 定义系数矩阵 A
A = [2, -3, 1;
     8, 3, 2;
     45, 1, -9];

% 2. 定义常数项列向量 b
b = [4; 2; 17];

% 3. 求解x
x = A\b;

disp('线性方程组的解 [x1; x2; x3] 为:');
disp(x);
```

## 6. $e^π$**与**$π^e$ **哪一个更大？有没有一种更一般的解决这类问题的办法？**

**(1) 直接计算法**
```matlab
val1 = exp(pi);
val2 = pi^exp(1);

fprintf('e^pi 的值约等于: %.10f\n', val1);
fprintf('pi^e 的值约等于: %.10f\n', val2);

if val1 > val2
    disp('结论: e^pi > pi^e');
else
    disp('结论: pi^e > e^pi');
end
```

从计算结果可以明确看到，eπ 的值大于 πe。

**(2) 通用分析法**

要找到一个更通用的方法来比较像 ab 和 ba 这样形式的数，我们可以构造一个函数来分析其单调性。

比较 eπ 和 πe 的大小，等同于比较 ln(eπ) 和 ln(πe) 的大小，即比较 πln(e) 和 eln(π)。

进一步变形，即比较 π 和 eln(π)，也等同于比较 fracln(e)e 和 fracln(π)π。

为此，我们构造函数 f(x)=fracln(x)x，其中 x0。

现在问题转化为比较 f(e) 和 f(π) 的大小。我们可以通过求导来判断函数的单调性。

f′(x)=fracfrac1xcdotx−ln(x)cdot1x2=frac1−ln(x)x2  

令 f′(x)=0，得到 1−ln(x)=0，解得 x=e。

- 当 0\<x\<e 时，$ \ln(x) < 1 $，所以 f′(x)0，函数 f(x) 在此区间单调递增。
    
- 当 xe 时，$ \ln(x) > 1 $，所以 f′(x)\<0，函数 f(x) 在此区间单调递减。
    

因此，函数 f(x) 在 x=e 处取得唯一极大值。

由于 πapprox3.14159eapprox2.71828，并且 π 和 e 都在函数的极大值点 x=e 的右侧或恰好在极大值点上，而函数在 (e,+infty) 区间上是单调递减的。

所以，f(π)\<f(e)。

即 fracln(π)π\<fracln(e)e。

两边同时乘以 eπ（这是一个正数），不等号方向不变：

eln(π)\<πln(e)

利用对数函数的性质 ln(ab)=bln(a)，得到：

ln(πe)\<ln(eπ)

因为 ln(x) 是一个单调递增函数，所以我们可以去掉两边的 ln，得到最终结论：

πe\<eπ

这个分析方法不仅解决了本题，也为比较任何两个形如 ab 和 ba 的数的大小提供了一种**普适性的解决思路**。

```matlab
% 定义核心函数 f(x) = ln(x)/x
f = @(x) log(x) ./ x;

% 获取 e 和 pi 的值
e_val = exp(1);
pi_val = pi;

% 计算 f(e) 和 f(pi)
f_e = f(e_val);
f_pi = f(pi_val);

% 创建x值范围
x = linspace(0.1, 5, 500);
y = f(x);

figure('Name', '函数单调性分析');

% 绘制函数曲线
plot(x, y, 'b-', 'LineWidth', 1.5, 'DisplayName', 'f(x) = ln(x)/x');
hold on;

% 在图上标记 e 和 pi 对应的点
scatter([e_val, pi_val], [f_e, f_pi], 60, 'r', 'filled', 'DisplayName', '关键点 (e, f(e)) 和 (\pi, f(\pi))');

% 添加垂直辅助线
xline(e_val, '--', {'x = e', sprintf('≈ %.4f', e_val)}, 'HandleVisibility', 'off');
xline(pi_val, '--g', {'x = \pi', sprintf('≈ %.4f', pi_val)}, 'HandleVisibility', 'off');

% 添加注释
text(e_val, f_e, sprintf('  f(e) ≈ %.4f (极大值点)', f_e), 'VerticalAlignment', 'bottom', 'HorizontalAlignment', 'left');
text(pi_val, f_pi, sprintf('  f(\pi) ≈ %.4f', f_pi), 'VerticalAlignment', 'top', 'HorizontalAlignment', 'left');

title('函数 f(x) = ln(x)/x 的图像与单调性分析', 'FontSize', 14);
xlabel('x', 'FontSize', 12);
ylabel('f(x)', 'FontSize', 12);
grid on; % 显示网格
legend('show', 'Location', 'northeast'); 
ax = gca; 
ax.XAxisLocation = 'origin'; % 将x轴移动到y=0的位置
ax.YAxisLocation = 'origin'; % 将y轴移动到x=0的位置
box off; 

hold off; % 释放图形保持状态
```