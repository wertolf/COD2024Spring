# 作业01解析

by 池雨泉

## 批改过程中发现的问题 & 注意事项

* 无论是选择题还是大题，请大家尽量写出**关键**的解题步骤或推断依据，如果过程过于简略，下次可能会酌情扣分
* 鼓励同学们尝试不同寻常的思路与解法
  * 新奇的思路或解法有机会被收录到作业解析中
* 计算过程中，最好带着分数或者根号，等到最后一步再转成小数，否则计算结果会出现较大的误差，例如习题1.5的小题b：
  ```
  >>> 18.18 * 2.6 / 7
  6.752571428571429
  >>> 18.18 * 2.64 / 7
  6.856457142857144
  ```
  * 如果想在过程中使用小数，也请保留较多的有效数字
* IPC与IPS是两个不同的指标
  * IPC - instructions per cycle
    $$\text{IPC} = \frac{1}{\text{CPI}}$$
  * IPS - instructions per second
    $$\text{IPS} = \text{r} \times \text{IPC} = \frac{\text{r}}{\text{CPI}}$$

### 请各位同学遵守最基本的学术诚信

个别同学的作业存在抄袭现象
* 下次如果再发现，该次作业直接记为零分
* 屡教不改者，本门课程成绩直接记为不及格

允许他人抄袭者与被抄袭者处罚相同

可以互相交流讨论，但不可以抄袭

### 关于单位的说明

* 时钟频率的单位
  * Hz (hertz)
  * kHz (kilohertz)
  * MHz (megahertz)
  * GHz (gigahertz)
* 每秒执行指令数的单位
  * IPS (instructions per second)
  * TIPS (thousands of instructions per second); kIPS (kilo instructions per second)
  * MIPS (millions of instructions per second)
  * BIPS (billions of instructions per second)

* 指令数以及时钟周期数一般不使用k/M/G或T/M/B这两个系列的缩写
* 不确定使用什么单位时，建议使用科学计数法，例如
  * $2 \times {10}^{10}$条指令
  * $3 \times {10}^{10}$个时钟周期

### 关于书写格式

* 在使用自己定义的符号前，请明确说明其含义
* 保持计算过程中所使用符号的统一性
* 科学计数法要求小数点的左边只有1位非零数字
  ```py
  20E9     # 不是科学计数法
  2E10     # 是科学计数法 (= 2 * (10 ** 10))
  0.43E10  # 不是科学计数法
  4.3E9    # 是科学计数法 (= 4.3 * (10 ** 9))
  ```

### 关于比值ratio、比重proportion与比率rate的说明

通常来说
* 比值 (ratio) 用于部分与部分之间的比较
* 比重 (proportion) 用于部分与整体之间的比较，用百分比 (percentage) 表示
* 比率 (rate) 是两种不同单位的量之间的商，分母通常与时间有关，用于表示速率或事件的发生率

因此，不要用百分比表示比值 (ratio)

中文上有时会将比值/比例/比率几个词混用，这时可以考虑借助英文来明确说明所使用的到底是哪个概念

当然，英文中也存在混用的情况，因此上面提到的区分方式不是绝对的

[拓展阅读](https://sphweb.bumc.bu.edu/otlt/MPH-Modules/PH717-QuantCore/PH717_BasicQuantitativeConcepts/PH717_BasicQuantitativeConcepts4.html)

## 单选第1题

答案：B

* A是不存在的机器
* B是对“存储程序”的阐述，正确
* C是与题干无关的选项
* D是相联存储器的特点

## 单选第2题

答案：D

软件和硬件具有逻辑功能上的等价性
* 硬件实现具有更高的执行速度
  * 执行频繁、硬件实现代价不是很高的功能通常由硬件实现
* 软件实现具有更好的灵活性

综上所述，D选项的说法是错误的

## 单选第3题

答案：B

以Linux上的C语言源程序的转换过程为例。流程如下：

* 预处理器 (preprocessor) 对代码中的`#include`/`#define`等宏 (macro) 进行解析，将以`.c`结尾的文件转换为以`.i`结尾的文件
* 编译器 (compiler) 将C语言程序翻译为汇编语言程序，生成以`.s`结尾的文件
* 汇编器 (assembler) 将汇编语言程序翻译为机器语言程序，生成以`.o`结尾的可重定位 (relocatable) 目标 (object) 文件
  * 可以进一步将若干目标文件整合，生成以`.a`/`.so`结尾的静态/动态库文件
* 链接器 (loader) 将若干目标文件与其所依赖的非标准库文件、标准库文件整合在一起，生成最后的可执行 (executable) 文件

Windows/Linux相关文件后缀名对比

| 文件类型 | Windows | Linux | 对应的英文 |
| ------- | ------- | ----  | -------- |
| C语言程序 | `.c` | `.c` | C |
| 预处理后的文件 | `.i` | `.i` | intermediate |
| 汇编语言程序 | `.asm` | `.s` | assembly language |
| 可重定位目标文件 | `.obj` | `.o` | object |
| 静态链接库 | `.lib` | `.a` | library; archive |
| 动态链接库 | `.dll` | `.so` | dynamic-link library; shared object |
| 可执行文件 | `.exe` | 无后缀名/任意后缀名 | executable |

## 单选第4题

答案：B

计算机的位数，即**机器字长**，指的是计算机能够一次性处理的二进制数的长度。一般情况下，通用寄存器的位数与机器字长相同，但是，这是两个不同的概念。因此选B。

## 单选第5题

答案：C

$$\text{执行时间t} = \frac{\text{指令数I} \times \text{平均每条指令所消耗的时钟周期数CPI}}{\text{时钟频率r}}$$

设
* 程序P在M1上的运行时间为 $\text{t}_1$
* 程序P在M2上的运行时间为 $\text{t}_2$

则
$$\frac{\text{t}_1}{\text{t}_2} = \frac{\frac{\text{I} \times \text{CPI}_1}{\text{r}_1}}{\frac{\text{I} \times \text{CPI}_2}{\text{r}_2}} = \frac{\text{CPI}_1}{\text{CPI}_2} \times \frac{\text{r}_2}{\text{r}_1} = \frac{1.5}{1} \times \frac{1.2}{1.5} = 1.2$$

## 单选第6题

答案：D

一个程序的总CPI等于其所使用的各种类型的指令的CPI按照其所占比例求得的加权平均

$$\text{CPI} = 2 \times 50\% + 3 \times 20\% + 2 \times 10\% + 1 \times 20\% = 2$$

$$\text{每秒百万条指令数MIPS} = \frac{\text{指令数I}}{\text{执行时间t} \times {10}^{6}}$$

而

$$\text{t} = \frac{\text{I} \times \text{CPI}}{\text{r}}$$

从而

$$\frac{\text{I}}{\text{t}} = \frac{\text{r}}{\text{CPI}}$$

从而

$$\text{MIPS} = \frac{\text{r}}{\text{CPI}} \times {10}^{-6} = \frac{1.2 \text{GHz}}{2} \times {10}^{-6} = 600$$

## 书后习题1.5

### 整理信息

| 处理器 | 时钟频率 | CPI |
|----|---------|-----|
| P1 |    3GHz | 1.5 |
| P2 |  2.5GHz | 1.0 |
| P3 |  4.0GHz | 2.2 |

### 小题a

* P1的IPS等于 $2 \times {10}^{9}$
* P2的IPS等于 $2.5 \times {10}^{9}$
* P3的IPS等于 $1.82 \times {10}^{9}$

就IPS而言，P2的性能最高。

#### 第一种理解方式

$$\text{每秒执行的指令数IPS} = \text{每秒的时钟周期数r} \times \text{平均每个周期所执行的指令数IPC} = \text{r} \times \frac{1}{\text{平均每条指令所消耗的时钟周期数CPI}}$$

#### 第二种理解方式

由于
$$\text{t} = \frac{\text{I} \times \text{CPI}}{\text{r}}$$
特别地，当 $\text{t}$ 为 $1$ 时
$$1 \text{秒} = \frac{\text{IPS} \times \text{CPI}}{\text{r}}$$
从而
$$\text{IPS} = \frac{1}{\frac{\text{CPI}}{\text{r}}} = \frac{\text{r}}{\text{CPI}}$$

#### 第三种理解方式

$$\text{每条指令所用时间} = \text{平均每条指令所消耗的时钟周期数CPI} \times \text{一个时钟周期所用时间T}$$
从而
$$\text{每条指令所用时间} = \text{CPI} \times \frac{1}{\text{时钟频率r}}$$
从而
$$\text{每秒钟所执行的指令数IPS} = \frac{1 \text{秒}}{\text{每条指令所用时间}} = \frac{\text{r}}{\text{CPI}}$$

### 小题b

| 处理器 | 时钟周期数 | 指令数 |
|----|------------------------|-------------------------|
| P1 |   $3 \times {10}^{10}$ |    $2 \times {10}^{10}$ |
| P2 | $2.5 \times {10}^{10}$ |  $2.5 \times {10}^{10}$ |
| P3 |   $4 \times {10}^{10}$ | $1.82 \times {10}^{10}$ |

#### 解法一

先求时钟周期数
$$\text{时钟周期数C} = \text{程序执行的秒数t} \times \text{每秒的时钟周期数r}$$
再求指令数
$$\text{指令数I} = \frac{\text{C}}{\text{CPI}}$$

#### 解法二

先求指令数
$$\text{指令数I} = \text{程序执行的秒数t} \times \text{每秒执行的指令数IPS}$$
再求时钟周期数
$$\text{时钟周期数C} = \text{I} \times \text{CPI}$$

#### 解法三

分别计算时钟周期数和指令数
$$\text{C} = \text{t} \times \text{r}$$
$$\text{I} = \text{t} \times \text{IPS}$$

### 小题c

$$\text{r}^\prime = \frac{\text{I} \times \text{CPI} \times (1 + 20\%)}{\text{t} \times (1 - 30\%)} = \frac{12}{7} \text{r}$$

改进后
* P1的频率为5.14GHz
* P2的频率为4.29GHz
* P3的频率为6.86GHz

## 书后习题1.9

### 整理信息

| 处理器型号 | 时钟频率 $F$ | 电压 $V$ | 静态功率 $P_s$ | 动态功率 $P_d$ |
| ------------------------- | ------- | ------ | ---- | ---- |
| Pentium 4 Prescott (2004) | 3.6 GHz | 1.25 V | 10 W | 90 W |
| Core i5 Ivy Bridge (2012) | 3.4 GHz |  0.9 V | 30 W | 40 W |

### 小题a

由动态功率的计算公式
$$P_d = \frac{1}{2} C V^2 F$$
得
$$C = \frac{2 P_d}{V^2 F}$$
代入数据
* Pentium 4 Prescott
$$C = \frac{2 \times 90\text{W}}{{(1.25)}^{2}\text{V}^{2} \times 3.6 \times {10}^{9}\text{Hz}} = 3.20 \times {10}^{-8}\text{F} \text{ 或者 } 32.00 \text{nF}$$

* Core i5 Ivy Bridge
$$C = \frac{2 \times 40\text{W}}{{(0.9)}^{2}\text{V}^{2} \times 3.4 \times {10}^{9}\text{Hz}} = 2.90 \times {10}^{-8}\text{F} \text{ 或者 } 29.05 \text{nF}$$

注：电容的单位是法拉 (F - farad)

### 小题b

| 处理器名称 | 静态功率所占百分比 | 静态功率与动态功率的比值 |
| ------------------ | ------ | ---- |
| Pentium 4 Prescott | 10.00% | 0.11 |
| Core i5 Ivy Bridge | 42.86% | 0.75 |

### 小题c

#### 结果

* Pentium 4 Prescott: 1.18 V
  ```
  >>> (math.sqrt(1600 + 4*288*450) - 40) / (2 * 288)
  1.1824830817583296
  ```
  * 与改进前相比降低了0.07伏
* Core i5 Ivy Bridge: 0.84 V
  ```
  >>> (math.sqrt(2700 ** 2 + 4 * 4000 * 63 * 81) - 2700) / (2 * 4000)
  0.8413368207686762
  ```
  * 与改进前相比降低了0.06伏

#### 解法一

以Core i5 Ivy Bridge为例

动态功率 $P_d$ 的计算公式
$$P_d = \frac{1}{2} C V^2 F$$

代入改进前的数据，求出 $C$ 与 $F$ 的乘积
$$40 = \frac{1}{2} \times {0.9}^2 C F$$
$$CF = \frac{2 \times 40}{0.81} = \frac{8000}{81} \text{F} \cdot \text{Hz}$$

静态功率 $P_s$ 的计算公式
$$P_s = V I$$

代入改进前的数据，求出电流 $I$
$$30 = 0.9 I$$
$$I = \frac{100}{3} \text{A}$$

计算改进后的总功率 $P^\prime$
$$P^\prime = (1 - 10\%) P = 0.9 \times (30 + 40) = 63 \text{W}$$

又因为总功率等于动态功率与静态功率之和
$$P^\prime = P_d^\prime + P_s^\prime$$

由于改进前后电流不变，改进后的静态功率为
$$P_s^\prime = \frac{100}{3} V^\prime$$

从而改进后的动态功率为
$$P_d^\prime = P^\prime - P_s^\prime = 63 - \frac{100}{3} V^\prime$$

又因为改进前后电容与频率均保持不变，因此
$$P_d^\prime = \frac{1}{2} C^\prime {V^\prime}^2 F^\prime = \frac{1}{2} C F {V^\prime}^2 = \frac{4000}{81} {V^\prime}^2$$

联立上述两式，整理得
$$4000 {V^\prime}^2 + 2700 V^\prime - 63 \times 81 = 0$$

解得
$$V^\prime = 0.84 \text{V}$$

#### 解法二

以Pentium 4 Prescott为例

由于
$$P_s = VI$$
$$P_d = \frac{1}{2}CV^2F$$
故
$$P_s \propto V$$
$$P_d \propto V^2$$
假设改进前后电压之比为$r$，即
$$r \stackrel{\text{def}}{=} \frac{V'}{V}$$
则有
$$P'_s = rP_s$$
$$P'_d = r^2P_d$$
又因为
$$P = P_s + P_d$$
$$P'= 0.9P$$
因此
$$rP_s + r^2P_d = 0.9(P_s + P_d)$$
代入数据，有
$$10r + 90r^2 = 90$$
解得
$$r = \frac{-1 + 5\sqrt{13}}{18}$$
因此
$$V' = rV = \frac{-1 + 5\sqrt{13}}{18} \times 1.25 = 1.18 \text{V}$$
