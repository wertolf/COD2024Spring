# 支持 24 条指令的单周期 MIPS CPU 设计

本实验的主电路位于 [cpu24.circ](../extra/hustzc/cpu24.circ) 文件中。

## 待回答的问题

* [ ] Logism 标准组件库中 ROM 组件和 RAM 组件有何区别

## 主要任务

* 构建数据通路
* 实现单周期硬布线控制器
* 软硬件测试联调

> [!NOTE]
> 实现控制器时，应输入相应的表达式，利用 Logisim 的`分析电路`功能**自动**生成电路。

## 关于 SYSCALL 指令的说明

```
input $a0, $v0

if $v0 == 34
    display value of $a0
else
    pause, wait for user pressing "Go" button
```

## 测试

在指令存储器中载入 `benchmark.hex` 镜像文件

使用 `Ctrl-K` 启动自动仿真

结束时
* 总周期数等于 1545
* 无条件分支数等于 38
* 条件分支成功数等于 276

> [!NOTE]
> 如果使用 MARS 模拟器运行该基准测试程序，最终的总周期数是 1546。

> [!IMPORTANT]
> 由于本实验需要完成一个支持 24 条指令的比较复杂的 CPU，在测试时，不应该一上来就运行基准测试程序，而应该针对所支持的 24 条指令编写一些简单的程序并检验其正确性之后，再来运行基准测试。
>
> 这是因为，如果直接运行基准测试，一旦出错，很难定位错误的原因。相反，一条一条指令地测试更容易排查故障。