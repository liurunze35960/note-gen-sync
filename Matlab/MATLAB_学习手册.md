欢迎！这份手册是为有 C++ 编程基础、并希望在生物信息学领域应用 MATLAB 的你量身定制的。我们将跳过冗长的语法讲解，直接进入实战。通过完成一系列精心设计的任务，你将不仅学会 MATLAB 的操作，更能理解其作为科学计算语言的强大之处——**矩阵思维**和**向量化**。

## **第一阶段：思维转变与基础核心**

**目标**：从 C++ 的“循环”思维，转向 MATLAB 的“矩阵”思维。

### **任务一：计算 DNA 序列的 GC 含量**

> 给定一条 DNA 序列（如 `'ATGCGCATTGAC'`），编写一个脚本计算其 G 和 C 的总数，并除以总长度。

**目标**：熟悉 MATLAB 的基本环境，并用向量化操作处理字符串。

**核心知识点**：

- **脚本（Script）**：在编辑器中编写 `.m` 文件。
    
- **变量与字符串**：MATLAB 中用单引号 `' '` 创建字符向量。
    
- **逻辑索引（Logical Indexing）**：使用 \=\= 对向量进行逻辑判断，会返回一个由 ` 0 ` 和 ` 1 ` 组成的逻辑向量。
    
- **向量化函数**：`sum()` 可以直接对向量的所有元素求和。`length()` 获取向量长度。
    

**详细解答**：

在 C++ 中，你可能会遍历字符串中的每个字符，用 `if` 语句判断它是否为 'G' 或 'C'，然后累加计数器。在 MATLAB 中，我们可以更直接地思考：

1. 找到所有 'G' 的位置。
    
2. 找到所有 'C' 的位置。
    
3. 将它们的数量相加。
    

```
% --- MATLAB 脚本：calculate_gc_content.m ---

% 清理工作区和命令行，这是一个好习惯
clear; 
clc;

% 1. 定义 DNA 序列 (字符向量)
dna_sequence = 'AGCTATAGCATCGGGTATGCCGTAGCT';

% 2. 向量化计算 GC 含量
% dna_sequence == 'G' 会返回一个逻辑向量，例如 [0 1 0 0 ...]
% 在 'G' 的位置是 1 (true)，其他位置是 0 (false)。
g_count = sum(dna_sequence == 'G');
c_count = sum(dna_sequence == 'C');

% 3. 计算总长度
total_length = length(dna_sequence);

% 4. 计算 GC 含量百分比
gc_content = ((g_count + c_count) / total_length) * 100;

% 5. 在命令行显示结果
% disp() 用于显示变量内容，fprintf() 用于格式化输出
fprintf('DNA 序列: %s\n', dna_sequence);
fprintf('GC 含量为: %.2f%%\n', gc_content);
```

**思维对比**：C++ 关注于“如何做”（how）——循环、判断、累加。MATLAB 关注于“做什么”（what）——“对整个序列寻找 G”、“对整个序列寻找 C”，代码即思想。

### **任务二：构建并使用氨基酸替换得分矩阵**

> 创建一个 5x5 的矩阵，模拟氨基酸替换得分。然后，给定两个等长的短肽链，通过矩阵索引，计算它们的替换得分总和。

**目标**：掌握 MATLAB 的核心——矩阵的创建、索引和运算。

**核心知识点**：

- **矩阵创建**：使用方括号 `[]` 创建矩阵，分号 `;` 用于换行。
    
- **矩阵索引**：使用 `(row, col)` 访问元素，**注意索引从 1 开始**。
    
- **冒泡索引（Colon Operator）**：`:` 可以代表某行或某列的所有元素。
    

**详细解答**：

假设我们需要一个简化的替换矩阵来计算两条短肽链的相似度得分。

```
% --- MATLAB 脚本：peptide_scoring.m ---

clear;
clc;

% 1. 定义氨基酸列表 (用 cell array 存储字符串)
amino_acids = {'A', 'R', 'N', 'D'};

% 2. 创建一个简化的 4x4 替换得分矩阵
%          A   R   N   D
score_matrix = [
     5, -2, -1, -2;  % A
    -2,  7, -1, -2;  % R
    -1, -1,  7,  2;  % N
    -2, -2,  2,  8   % D
];

% 3. 定义两条需要比对的肽链
peptide1 = 'ANDR';
peptide2 = 'AADN';

% 4. 计算比对得分 (这里使用循环是为了逻辑清晰，后续会展示更高级的方法)
total_score = 0;
for i = 1:length(peptide1)
    % 获取当前位置的氨基酸
    aa1 = peptide1(i);
    aa2 = peptide2(i);
    
    % 找到氨基酸在列表中的索引
    % find(strcmp(cell_array, string)) 是查找字符串在 cell array 中位置的常用方法
    idx1 = find(strcmp(amino_acids, aa1));
    idx2 = find(strcmp(amino_acids, aa2));
    
    % 从得分矩阵中提取得分并累加
    current_score = score_matrix(idx1, idx2);
    total_score = total_score + current_score;
    
    fprintf('比对 %c 和 %c, 得分: %d\n', aa1, aa2, current_score);
end

fprintf('\n两条肽链的总比对得分为: %d\n', total_score);
```

**思维对比**：在 C++ 中，你需要定义一个二维数组。在 MATLAB 中，矩阵是“一等公民”，创建和访问都极其自然。这个例子虽然用了循环，但核心操作 `score_matrix(idx1, idx2)` 体现了矩阵作为数据查找表的强大能力。

## **第二阶段：生物数据处理与可视化**

**目标**：学会处理真实的表格数据，并用图形化方式探索其规律。

### **任务三：处理基因表达数据**

> 找一份公开的基因表达谱数据（例如，某些癌症样本和正常样本的基因表达数据）。用 `readtable` 将其读入 MATLAB。然后，练习筛选出某个特定基因在所有样本中的表达数据，或者筛选出表达量大于某个阈值的所有基因。

**目标**：从外部文件（如 .csv）读取数据，并使用 `table` 数据类型进行筛选。

**核心知识点**：

- **创建数据文件**：学会手动创建一个简单的 `.csv` 文件。
    
- **读取表格**：使用 `readtable` 函数。
    
- **Table 数据类型**：MATLAB 中用于处理异构列数据（字符串、数字等混合）的强大工具。
    
- **Table 索引**：使用 `.` 符号（如 `T.GeneName`）或 `{}` 和 `()` 进行数据提取和筛选。
    

**详细解答**：

第一步：创建数据文件

请在你的电脑上创建一个名为 gene_expression.csv 的文本文件，并将以下内容复制进去：

```matlab
GeneName,Control_1,Control_2,Cancer_1,Cancer_2
TP53,10.5,11.2,5.3,6.1
EGFR,8.9,9.1,15.8,16.2
BRCA1,12.1,11.8,7.5,7.9
MYC,9.5,10.1,18.2,17.9
```

**第二步：编写 MATLAB 脚本读取和处理数据**

```
% --- MATLAB 脚本：process_expression_data.m ---
clear;
clc;

% 1. 读取 CSV 文件到 Table 中
% 'PreserveVariableNames',true 确保列名和文件中的完全一致
opts = detectImportOptions('gene_expression.csv');
opts = setvartype(opts, 'GeneName', 'string'); % 将基因名读取为 string 类型
T = readtable('gene_expression.csv', opts);

% 2. 显示整个表格
disp('原始基因表达数据:');
disp(T);

% 3. 数据筛选示例
% 示例 A: 提取 EGFR 基因的所有表达数据
% T.GeneName == "EGFR" 会返回一个逻辑向量，用于选择行
egfr_row = T(T.GeneName == "EGFR", :);
disp('EGFR 基因的表达数据:');
disp(egfr_row);

% 示例 B: 提取所有基因在癌症样本中的表达数据
% 使用变量名（列名）来选择列
cancer_data = T(:, ["GeneName", "Cancer_1", "Cancer_2"]);
disp('所有基因在癌症样本中的数据:');
disp(cancer_data);
```

**思维对比**：相比 C++ 需要文件流、字符串分割等一系列繁琐操作，MATLAB 的 `readtable` 一行代码就完成了数据的结构化载入，并且 `table` 类型使得后续按名索引数据变得极其方便和直观。

### **任务四：基因表达数据的差异分析与可视化**

**目标**：对处理后的数据进行简单的统计分析，并用图表展示结果。

**核心知识点**：

- **数据提取**：从 `table` 中提取数值向量。
    
- **基础统计**：`mean()`（均值），`std()`（标准差）。
    
- **绘图函数**：`bar()`（柱状图），`errorbar()`（误差棒），`ylabel()`，`title()`，`legend()`。
    
- **统计检验**：`ttest2()`（双样本 t 检验）。
    

**详细解答**：

我们将接着上一个任务的数据，比较 `EGFR` 基因在对照组和癌症组的表达差异。

```
% --- MATLAB 脚本：visualize_expression_data.m ---

% (请确保先运行了上一个任务的脚本，或者将读取部分复制到这里)
clear; clc;
opts = detectImportOptions('gene_expression.csv');
opts = setvartype(opts, 'GeneName', 'string');
T = readtable('gene_expression.csv', opts);

% 1. 提取 EGFR 基因的数据
egfr_row = T(T.GeneName == "EGFR", :);

% 将 table 中的数据行转换为数值数组，方便计算
control_values = [egfr_row.Control_1, egfr_row.Control_2];
cancer_values = [egfr_row.Cancer_1, egfr_row.Cancer_2];

% 2. 计算均值和标准差
mean_control = mean(control_values);
mean_cancer = mean(cancer_values);
std_control = std(control_values);
std_cancer = std(cancer_values);

% 3. 进行 t-检验
% h=1 表示拒绝原假设（两组均值相等），p 是 p-value
[h, p] = ttest2(control_values, cancer_values);

fprintf('EGFR 基因在两组间的 t-检验结果:\n');
fprintf('P-value: %.4f\n', p);
if h
    fprintf('结论: 在统计学上存在显著差异。\n\n');
else
    fprintf('结论: 在统计学上不存在显著差异。\n\n');
end

% 4. 可视化
% 创建一个新的图形窗口
figure; 

% 绘制柱状图
bar_data = [mean_control, mean_cancer];
b = bar(bar_data);
hold on; % 保持当前图形，以便添加误差棒

% 添加误差棒
errorbar(1:2, bar_data, [std_control, std_cancer], '.k', 'LineWidth', 1.5);

hold off; % 释放图形

% 5. 美化图表
set(gca, 'XTickLabel', {'Control', 'Cancer'}); % 设置 X 轴刻度标签
ylabel('基因表达量 (FPKM)');
title('EGFR 基因在不同样本中的表达量');
legend({'平均表达量', '标准差'});
grid on; % 添加网格线
```

**运行结果**：脚本会输出 t-检验的 p-value，并弹出一个柱状图，清晰地展示 EGFR 在癌症组中的表达量显著高于对照组。

## **第三阶段：数学建模与专业应用**

**目标**：使用 MATLAB 作为数学工具，解决生物学中的建模问题。

### **任务五：模拟 Hardy-Weinberg 平衡**

**目标**：学习编写自定义函数，并使用循环进行简单的模拟。

**核心知识点**：

- **函数（Function）**：创建以 `function` 关键字开头的 `.m` 文件，它有独立的输入和输出。
    
- **随机数**：`rand()` 生成 (0,1) 之间的随机数。
    
- **循环与逻辑**：`for` 循环和 `if-else` 语句。
    

**详细解答**：

第一步：创建函数文件

创建一个名为 run_hw_simulation.m 的新文件，并写入以下代码。

```
function [p_history, q_history] = run_hw_simulation(p0, population_size, generations)
% run_hw_simulation: 模拟Hardy-Weinberg平衡过程
% 输入:
%   p0: 等位基因 'A' 的初始频率
%   population_size: 种群大小
%   generations: 模拟的代数
% 输出:
%   p_history: 每一代 'A' 基因频率的历史记录
%   q_history: 每一代 'a' 基因频率的历史记录

    % 初始化
    p = p0;
    q = 1 - p0;
    p_history = zeros(1, generations + 1);
    q_history = zeros(1, generations + 1);
    p_history(1) = p;
    q_history(1) = q;

    % 开始模拟每一代
    for g = 1:generations
        
        % 根据当前基因频率，生成下一代
        next_gen_alleles = '';
        for i = 1:(2 * population_size) % 每个个体有两个等位基因
            if rand() < p
                next_gen_alleles = [next_gen_alleles, 'A'];
            else
                next_gen_alleles = [next_gen_alleles, 'a'];
            end
        end
        
        % 计算新一代的基因频率
        p = sum(next_gen_alleles == 'A') / (2 * population_size);
        q = 1 - p;
        
        % 记录历史
        p_history(g + 1) = p;
        q_history(g + 1) = q;
    end
end
```

第二步：创建脚本文件来调用函数并绘图

再创建一个名为 test_hw.m 的脚本文件。

```
% --- MATLAB 脚本：test_hw.m ---
clear; clc;

% 1. 设置模拟参数
initial_p = 0.2;       % 'A' 基因的初始频率
pop_size = 1000;       % 种群大小
num_generations = 100; % 模拟100代

% 2. 调用我们编写的函数
[p_hist, q_hist] = run_hw_simulation(initial_p, pop_size, num_generations);

% 3. 可视化结果
figure;
plot(0:num_generations, p_hist, '-b', 'LineWidth', 2);
hold on;
plot(0:num_generations, q_hist, '-r', 'LineWidth', 2);
hold off;

% 4. 美化图表
ylim([0 1]); % 设置 Y 轴范围
xlabel('代数 (Generations)');
ylabel('等位基因频率');
title('Hardy-Weinberg 平衡模拟');
legend({'p (频率 of A)', 'q (频率 of a)'});
grid on;
```

**运行结果**：运行 `test_hw.m`，你会看到一条蓝线和一条红线，它们在初始值附近轻微波动，但总体保持稳定，直观地展示了遗传平衡。

### **任务六：建立一个简单的酶促反应动力学模型**

**目标**：体验 MATLAB 在科学计算中的核心优势——求解常微分方程（ODE）。

**核心知识点**：

- **常微分方程（ODE）**：如何用函数描述一个系统的动态变化。
    
- **ODE 求解器**：`ode45` 是最常用的标准求解器。
    
- **函数句柄（Function Handle）**：使用 `@` 符号将一个函数作为另一个函数的输入参数。
    

**详细解答**：

米氏方程描述了酶(E)和底物(S)结合成复合物(ES)，再分解成产物(P)和酶的过程。

E + S <--> ES --> E + P

这可以用一个微分方程组来描述。

第一步：创建定义微分方程的函数

创建一个名为 enzyme_kinetics.m 的函数文件。

```
function dydt = enzyme_kinetics(t, y, k1, k_minus_1, k2)
% enzyme_kinetics: 定义酶促反应的常微分方程组
% t: 时间 (ode45需要，虽然此方程中未使用)
% y: 状态变量向量 y(1)=[S], y(2)=[E], y(3)=[ES], y(4)=[P]
% k1, k_minus_1, k2: 反应速率常数

    % 从输入向量 y 中分解出各个物质的浓度
    S = y(1);
    E = y(2);
    ES = y(3);
    % P = y(4); % P的浓度变化可以由其他推导，这里暂时不用

    % 初始化输出向量 dydt (浓度的变化速率)
    dydt = zeros(4, 1);
    
    % 定义每个物质浓度随时间的变化率
    dydt(1) = -k1 * E * S + k_minus_1 * ES;      % d[S]/dt
    dydt(2) = -k1 * E * S + (k_minus_1 + k2) * ES; % d[E]/dt
    dydt(3) = k1 * E * S - (k_minus_1 + k2) * ES;  % d[ES]/dt
    dydt(4) = k2 * ES;                           % d[P]/dt
end
```

第二步：创建脚本来求解并绘图

创建一个名为 run_enzyme_model.m 的脚本文件。

```
% --- MATLAB 脚本：run_enzyme_model.m ---
clear; clc;

% 1. 定义模型参数
k1 = 100;        % E+S -> ES 的速率
k_minus_1 = 1;   % ES -> E+S 的速率
k2 = 10;         % ES -> E+P 的速率

% 2. 定义初始条件 [S0, E0, ES0, P0]
y0 = [10, 2, 0, 0]; % 初始底物浓度10，酶浓度2，其他为0

% 3. 定义求解的时间范围
t_span = [0, 5]; % 从 0 到 5 秒

% 4. 使用 ode45 求解
% @(t,y) 创建一个匿名函数，将参数 k1, k_minus_1, k2 固定传入
[t, y] = ode45(@(t,y) enzyme_kinetics(t, y, k1, k_minus_1, k2), t_span, y0);

% 5. 可视化结果
figure;
plot(t, y(:,1), 'LineWidth', 2); % 底物 S
hold on;
plot(t, y(:,2), 'LineWidth', 2); % 酶 E
plot(t, y(:,3), 'LineWidth', 2); % 复合物 ES
plot(t, y(:,4), 'LineWidth', 2); % 产物 P
hold off;

% 6. 美化图表
xlabel('时间 (s)');
ylabel('浓度');
title('酶促反应动力学模拟');
legend({'[S] - 底物', '[E] - 酶', '[ES] - 复合物', '[P] - 产物'});
grid on;
```

**运行结果**：运行 `run_enzyme_model.m`，你将得到四条曲线，清晰地展示了底物被消耗、产物逐渐生成、以及中间复合物浓度先上升后下降的动态过程。这充分体现了 MATLAB 在动态系统建模中的强大能力。

## **第四阶段：综合项目与工具箱探索**

当你完成了以上所有任务，你已经掌握了 MATLAB 的核心思想和常用操作。接下来，你可以挑战更综合的项目，并开始探索 MATLAB 强大的工具箱生态。

- **综合项目建议**：
    
    - **基因调控网络模拟**：使用 ODE 模拟一个简单的基因激活-抑制网络。
        
    - **简单的系统发育树构建**：读取多序列比对文件，计算距离矩阵，用 UPGMA 算法构建树。
        
    - **生物图像细胞计数**：利用图像处理技术自动识别并计数显微镜图片中的细胞。
        
- **推荐探索的工具箱 (Toolbox)**：
    
    - **Bioinformatics Toolbox™**：提供了海量的专业函数，用于序列分析、基因组学、蛋白质组学等，能极大简化你的工作。
        
    - **Statistics and Machine Learning Toolbox™**：进行更复杂的统计分析、回归、分类和聚类。
        
    - **Image Processing Toolbox™**：处理和分析生物图像。
        

### **给 C++ 程序员的最终建议**

1. **拥抱向量化**：在写 `for` 循环之前，先花十秒钟思考：“这个操作能用一次矩阵或向量运算完成吗？”
    
2. **索引从 1 开始**：牢记 MATLAB 的数组索引从 `1` 开始。
    
3. **善用 `help` 和 `doc`**：在命令行输入 `help function_name` 或 `doc function_name`，可以获得比任何教程都详细的官方文档。
    

祝你学习愉快，享受 MATLAB 带来的高效与便捷！