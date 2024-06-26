# 支持 8 条指令的单周期 MIPS CPU 设计

## 实验任务

* 绘制数据通路
* 实现单周期硬布线控制器
* 测试联调

## 核心指令集

* [ ] 什么是 RTL 功能描述

各指令的 opcode/funct 字段可通过查阅 [MIPS Green Sheet](../extra/hustzc/MIPS_Green_Sheet.pdf) 获得。

* add 指令
  * opcode 为 0
  * funct 为 0x20
* slt 指令
  * opcode 为 0
  * funct 为 0x2a
* addi 指令
  * opcode 为 0x08
* lw 指令
  * opcode 为 0x23
* sw 指令
  * opcode 为 0x2b
* beq 指令
  * opcode 为 0x4
* bne 指令
  * opcode 为 0x5
* syscall 指令
  * opcode 为 0
  * funct 为 0x0c

不要求实现异常处理

## 步骤 1 - 绘制数据通路

> [!TIP]
> [CS3410](../extra/hustzc/cs3410.jar) 组件库中的寄存器文件可以实时显示各寄存器的值，不过该组件所占面积较大，因此华科计组教研组对该组件进行了二次封装。同学们可以根据自己的习惯进行选择。

## 步骤 2 - 实现控制器

为了简化绘图工作量、将重点放在对 CPU 工作原理的理解上，可以使用比较器实现非 R 型指令的译码逻辑。但是需要注意，实际的电路设计中为了提高性能、降低成本，不会采用这种方式。

### 通过 opcode 和 funct 产生 ALU_OP

不需要为 syscall 指令设置 ALU_OP 的值

由于 ALU 提供了 Equal 的输出，因此不需要为 beq 和 bne 指令设置 ALU_OP 的值

于是只需要考虑 5 条指令和 2 个运算，因此比较容易，用纸和笔即可确定电路逻辑

> [!NOTE]
> 这里记录一下我的调试心路历程。
>
> 一开始我觉得只需使用 funct 字段的第 1 号比特（从 0 开始编号）即可，结果运行排序测试程序时，发现第一条 addi 指令的 ALU_OP 值竟然就不是加法。于是猛然发现 I 型指令没有 funct 字段！相应的位置是 imm 字段的低 6 位。显然 imm 字段低 6 位的内容是不可控的，因此需要寻找其他的实现方案。
>
> 解决方法其实很简单：由于只有 slt 指令需要的 ALU_OP 的值是 1011，其余 4 条指令所需要的 ALU_OP 均是加法的 0101，因此只需要用前面比较器生成的 SLT 控制信号控制一个多选器即可。

* [ ] 阅读理论课教材附录 D，了解如何系统地构建这类电路

## 步骤 3 - 运行冒泡排序测试程序

注意在停机的时候
* 不能对时钟直接进行与操作，把时钟停掉
* 要求大家控制相应寄存器的写使能信号来达到停机的目的

换句话说，在进行停机控制的时候，应该控制电平信号，而不是时钟信号。

* [ ] 停机时报错 oscillation apparent 是否是正常现象？
