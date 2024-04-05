# 作业02解析

by 李兆坤

## 作业的主要问题

**CPU如何执行计算**

部分同学可能对CPU如何执行计算不太理解，具体可以参考 [2.10](#210) 的解析和课本相关内容

**汇编语言到高级语言**

部分同学按照汇编语言的逻辑，基本是逐句翻译为高级语言，这样会失去高级语言的简洁性（虽然也是正确的）

当然，高级语言的抽象也要基于原本的汇编语言，不能改变其原意：比如循环的判断条件是 `i<10`，最好使用 `slt` 实现小于比较分支，不要直接用 `i=10` 作为跳出循环的标志

**循环过程的汇编代码**

具体可以参考 [2.25](#225) 的解析和课本相关内容

**过程调用**

很多同学对硬件如何支持过程（如函数）调用不太理解，以及过程调用中用到的32个通用寄存器的作业也有些混淆，具体可以参考 [2.31](#231) 的解析和课本相关内容

**关于课本参考答案**

部分参考答案有明显错误，在参考时注意自己的思考

## 2.1

```mipsasm
addi $s0, $s2, -5
add $s0, $s0, $s1
```

也可以不使用寄存器表示：

```mipsasm
addi f, h, -5  
add f, f, g
```

**注意：没有 `subi`**

## 2.4

```c
B[g]= A[f] + A[f+1]
```

```mipsasm
sll $t0, $s0, 2 # t0 = f * 4
add $t0, $s6, $t0 # t0 = &A[f]
lw $t1, 0($t0) # s0 = A[f]
lw $t2, 4($t0) # s1 = A[1+f]
add $t0, $t1, $t2 # t0 = A[f] + A[1+f]
sll $t1, $s1, 2 # t1 = g * 4
add $t1, $s7, $t1 # t1 = &B[g]
sw $t0, 0($t1) # B[g] = A[f] + A[1+f]
```

## 2.10

### 2.10.1

0x8000 0000 0000 0000 + 0xD000 0000 0000 0000
=0x1 5000 0000 0000 0000

最高位 1 溢出

故 $t0 中的值为：0x5000 0000 0000 0000

> 有符号数和无符号数都是人为规定的，计算机在执行运算时并不区分。在计算加法时，都是从最低位加，进位。
>
> 寄存器里不会存储 `-` 等符号，都是 01 值。

### 2.10.2 

溢出后的结果

> *课本2.4，P56*
>
> 溢出是指一个操作的结果过大，以至于不能使用一个寄存器表示。如何处理溢出是软件决定的事，硬件只会计算和提供溢出检测（比如溢出位置 1）。
>
> *课本3.2 P135*
>
> 课本上提供一种用MIPS指令序列检测溢出的方法，这里的溢出是平常使用较多的溢出，就是超过一种数（有符号数或无符号数）的表示范围，注意在不同语境下，与上面的溢出定义（**操作数溢出**）区别。
>
> 其实，所谓 `add` 和 `addu`，在执行加法运算上没什么区别，只不过前者会进行溢出检测（课本2.4，P56所定义的溢出）：在发生溢出时会抛出异常，运算结果并不会存储到目的寄存器。这也是为什么这里使用 `addu` 进行有/无符号数的**数值范围溢出**检测，因为不会抛出异常，运算结果都会存储到目的寄存器。
>

### 2.10.3

0x8000 0000 0000 0000 - 0xD000 0000 0000 0000
=0x8000 0000 0000 0000 + (- 0xD000 0000 0000 0000)
=0x8000 0000 0000 0000 + 0x3000 0000 0000 0000 (按位取反+1)
=0xB000 0000 0000 0000

> 计算机实际上进行的是补码运算，减法一般会转换为加法计算。
>
> 课本P57: 快速求相反数
>

### 2.10.4

期望的结果

### 2.10.5 

0x8000 0000 0000 0000 + 0xD000 0000 0000 0000
=0x5000 0000 0000 0000

0x5000 0000 0000 0000 + 0x8000 0000 0000 0000
=0xD000 0000 0000 0000

故 $t0 中的值为：0xD0000000

#### 2.10.6 

溢出后的结果

> 第一步就溢出了
>

## 2.12

| R型 | rs = $s0 | rt = $s0 | rd = $s0| shamt = 0 | funct = 32: add|
|----|----|----|----|----|----|
| 0000000 | 10000 | 10000 | 10000 | 00000 | 100000 |

R -type: `add $s0, $s0, $s0`

## 2.13

| I型: sw | rs = $t2 | rt = $t1 | offset = 32|
|----|----|----|----|
| 101011 | 01010 | 01001 | 0000000000100000 |

0b1010 1101 0100 1001 0000 0000 0010 0000
=0xAD49 0020

## 2.24

### 2.24.1

20

### 2.24.2

```C
i = 10;
do {
    i = i – 1;
    B += 2;
} while (i > 0)
```

### 2.24.3

5*N+2.

循环体内部 5 条指令，最后跳出循环还需要执行 2 条

## 2.25

```mipsasm
addi $t0, $0, 0 # 初始化 i=0
beq $0, $0, TEST1 # 判断条件复杂，跳转分支执行
LOOP1:
addi $t1, $0, 0 # 初始化 j=0
beq $0, $0, TEST2
LOOP2:
add $t3, $t0, $t1
# 从内存中取得 D[4*j]地址给$t2
sll $t2, $t1, 4 # 逻辑左移快速乘法：24=16=4*4
add $t2, $t2, $s2
# 将 i+j 存入 D[4*j]
sw $t3, ($t2)
addi $t1, $t1, 1 # ++j
TEST2:
slt $t2, $t1, $s1
bne $t2, $0, LOOP2
addi $t0, $t0, 1 # ++i
TEST1:
slt $t2, $t0, $s0
bne $t2, $0, LOOP1
```

> 当判断条件复杂时（嵌套、不允许使用伪指令等），可以用恒成立跳转至一个专门实现判断的标签。
>
> 循环嵌套一般从循环体到判断条件、从内到外书写。 这样写的好处是： 当跳出循环时（分支不发生），可以直接执行接下来的指令，而无需额外的跳转。
>

可以对比另一种翻译：

```mipsasm
addi $t0, $0, 0
loop1:
slt $t2, $t0, $s0
beq $t2, $0, exit1
addi $t1, $0, 0
loop2:
slt $t2, $t1, $s1 
beq $t2, $0, exit2
add $t3, $t0, $t1
sll $t2, $t1, 4
add $t2, $t2, $s2
sw $t3, 0($t2)
addi $t1, $t1, 1
j loop2
exit2:
addi $t0, $t0, 1
j loop1
exit1:

```

> 对比发现，两段代码分别使用不满足则分支（`bne`）和满足则分支（`beq`），虽然都正确，但可能执行效率上会有所不同。For循环只会有一次不满足的情况，就是跳出循环时，如果每次满足条件都要分支，效率会很低，故采用不满足则分支更好。同样的情况也常见于 `if-else` 和 `while` 语句中。
>

## 2.26

(1) 14条

(2) 答案因指令序列的不同而不同，但大致在150左右

- 对于 2.25 中的第一种翻译：144条

初始 2条

LOOP2 5条，TEST2 2条，跳出循环时 3条，b=1循环1次

LOOP1 2条，TEST1 2条，跳出循环时 2条，a=10循环10次

共计 2 + 10 * (2 + 1 * (5 + 3 + 2) + 2) + 2 = 144

- 对于第二种翻译，同样计算可得 1 + 10 * (3 + 1 * (8 + 4)) + 2 = 153

如果想要检验自己指令序列对应的答案是否正确，可以写脚本模拟验证，例如使用 Python:

```python
class MIPS_Simulator:
    def __init__(self, instructions, registers):
        self.instructions = instructions
        self.registers = registers
        self.pc = 0
        self.instruction_count = 0

    def execute(self):
        while self.pc < len(self.instructions):
            instruction = self.instructions[self.pc]
            opcode = instruction[0]
            label = False

            if opcode == 'ADD':
                self.registers[int(instruction[1])] = self.registers[int(instruction[2])] + self.registers[int(instruction[3])]
            elif opcode == 'ADDI':
                self.registers[int(instruction[1])] = self.registers[int(instruction[2])] + int(instruction[3])
            elif opcode == 'SUB':
                self.registers[int(instruction[1])] = self.registers[int(instruction[2])] - self.registers[int(instruction[3])]
            elif opcode == 'MUL':
                self.registers[int(instruction[1])] = self.registers[int(instruction[2])] * self.registers[int(instruction[3])]
            elif opcode == 'DIV':
                self.registers[int(instruction[1])] = self.registers[int(instruction[2])] // self.registers[int(instruction[3])]
            elif opcode == 'BEQ':
                if self.registers[int(instruction[1])] == self.registers[int(instruction[2])]:
                    self.pc = self.instructions.index([instruction[3] + ':']) - 1
            elif opcode == 'BNE':
                if self.registers[int(instruction[1])] != self.registers[int(instruction[2])]:
                    self.pc = self.instructions.index([instruction[3] + ':']) - 1
            elif opcode == 'JUMP':
                self.pc = self.instructions.index([instruction[1] + ':']) - 1
            elif opcode == 'SLL':
                self.registers[int(instruction[1])] = self.registers[int(instruction[2])] << int(instruction[3])
            elif opcode == 'SLT':
                if self.registers[int(instruction[2])] < self.registers[int(instruction[3])]:
                    self.registers[int(instruction[1])] = 1
                else:
                    self.registers[int(instruction[1])] = 0
            elif opcode == 'SW':
                print('Saved Word')
            else:
                label = True

            self.pc += 1
            if label == False:
                self.instruction_count += 1

    def get_instruction_count(self):
        return self.instruction_count

# 示例指令序列
instructions = [
    ['ADDI', 8, 0, 0],         # $t0 = 0
    ['BEQ', 0, 0, 'TEST1'],    # branch to TEST1
    ['LOOP1:'],
    ['ADDI', 9, 0, 0],         # $t1 = 0
    ['BEQ', 0, 0, 'TEST2'],    # branch to TEST2
    ['LOOP2:'],
    ['ADD', 11, 8, 9],         # $t3 = $t0 + $t1
    ['SLL', 10, 9, 4],         # $t2 = $t1 << 4
    ['ADD', 10, 10, 18],       # $t2 = $t2 + $s2
    ['SW', 11, 10],            # memory[$t2] = $t3
    ['ADDI', 9, 9, 1],         # $t1++
    ['TEST2:'],
    ['SLT', 10, 9, 17],        # $t2 = ($t1 < $s1)
    ['BNE', 10, 0, 'LOOP2'],   # branch to LOOP2 if $t2 != 0
    ['ADDI', 8, 8, 1],         # $t0++
    ['TEST1:'],
    ['SLT', 10, 8, 16],        # $t2 = ($t0 < $s0)
    ['BNE', 10, 0, 'LOOP1']    # branch to LOOP1 if $t2 != 0
]

# 初始寄存器值
registers = [0] * 32
registers[16] = 10  # $s0
registers[17] = 1   # $s1
registers[18] = 100 # $s2

simulator = MIPS_Simulator(instructions, registers)
simulator.execute()

print("Total instructions executed:", simulator.get_instruction_count())

```


## 2.31

```mipsasm
f: # 注意函数定义标签
# protect/save stack:
addi $sp,$sp,-12
sw $ra,8($sp) # 返回地址肯定需要被存储保护
# 因为过程要使用到 $s1 和 $s0
sw $s1,4($sp)
sw $s0,0($sp)

# $a0:a, $a1:b, $a2:c, $a3:d
move $s1,$a2 # 伪指令：把后者赋值给前者
move $s0,$a3 
jal func
move $a0,$v0 # func(a, b)
add $a1,$s0,$s1 # c + d
jal func

# recover
lw $ra,8($sp)
lw $s1,4($sp)
lw $s0,0($sp)
addi $sp,$sp,12

jr $ra # 将控制权还给主程序
```

**过程调用中栈的作用**

过程调用中栈起保护作用，保护那些*可能被过程修改、但调用者还需要*（两个方面）的寄存器中的**值**（注意是保护值，而非寄存器本身，pop/push 栈操作也是在操作寄存器值）。

最典型的是对 `$ra` 寄存器的保护，因为 `jal` 指令（跳转到某个地址同时把返回调用点的地址存储在 `$ra` 中）会修改 `$ra` 中的值。

在本题中，两条 `move` 伪指令修改了寄存器 `$s1` 和 `$s0` 的值，所以需要对其进行保护。MIPS 中约定，`$s` 寄存器均为保留（save）寄存器，必须进行保护；`$t` 寄存器均为临时（temp）寄存器，不必进行保护。由于栈操作消耗较大，一般优先使用临时寄存器。本题如果使用 `$t`，则无需对其进行栈保护。

值得注意的是参数寄存器，这需要联系函数传参的过程。假设有如下 C 语句:

```c
void add(a, b){
    printf("%d\n", a+b);
}

int x = 1;
int y = 2;
add(x, y);

```

学习 C 语言时，我们知道这种参数传递是“值传递”，相当于在调用函数过程中执行了 `a=x;b=y;`，这样在函数内部的局部变量a和b对调用者的x和y就没有影响。

这正是一般函数调用时的操作，先把参数寄存器赋值，再调用（本题中也有类似操作）。即便在函数内，参数寄存器的值被修改了（如本题），但对调用者没有任何影响，所以也无须对参数寄存器的值进行栈保护。

但并不是所有函数调用都不需要保护参数寄存器的值，考虑求阶乘的递归函数：

```C
int fact(n){
    if (n==1){
        return n;
    }
    else{
        return n*fact(n-1);
    }
}

```

在执行 `return n*fact(n-1);` 时，`$a0` 中是n的值，当然可以为 n-1 再分配一个参数寄存器 `$a1`。但问题是：递归函数的执行次数是不定的；当n很大时，参数寄存器（甚至32个寄存器）也不够用；如果修改了 `$a0`，则在 `fact(n-1)` 返回时，不能正确计算 `return n*fact(n-1);`。

所以，我们在每次递归时，需要对 `$a0` 中的值进行保护。同时，也可以看出，为什么递归实现的代码简洁但运行效率低，因为递归不断进行过程调用，而栈操作实际上是进行内存操作，内存的传输速度要远低于CPU的计算速度。如果是迭代实现，只是在cpu和寄存器上计算，会快很多。
