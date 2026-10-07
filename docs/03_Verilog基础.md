## 1.学习主线
Verilog在描述什么 → module模块 → input/output → 位宽与二进制表示 → wire/reg → assign组合逻辑 → always过程块 → 阻塞赋值=与非阻塞赋值<= → if/case → 计数器 → 分频 → PWM → 状态机 → J280电机控制

直接对应这J280：Verilog逻辑 → FPGA内部LUT/FF → PWM与方向控制 → TB6612 → 电机 → 编码器/ADC反馈 → 姿态控制

## 2. Verilog是什么
你以前学单片机时，C语言主要是在告诉 CPU：
按照什么顺序执行哪些指令。

例如：
a = b & c;

可以理解成 CPU 执行到这一行时，计算一次 b & c。
但是 Verilog 的思维不一样。
例如：
assign a = b & c;

它更接近于：
在 FPGA 内部构造一个与门，把 b、c 接到输入，把输出接到 a。

Verilog不是在描述“程序执行过程”，而是在描述“数字电路结构和行为”。

## 3.module：一个硬件模块
Verilog最基本的单位叫：module
![alt text](image-4.png)

例如：

```verilog
module key_led (
    input  wire key,
    output wire led
);

assign led = key;

endmodule
```

这里：
module key_led
表示创建了一个名字叫：
key_led
的数字电路模块。
可以把它画成：
```text
          key_led
      ┌─────────────┐
key ─→│             │─→ led
      └─────────────┘
```
所以：
- module：模块开始
- endmodule：模块结束
以后一个复杂系统通常就是很多 module 拼起来的

![alt text](image-5.png)
![alt text](image-6.png)
## 4. module的端口声明部分
![alt text](image-7.png)
![alt text](image-8.png)
- wire是数字信号线。output wire led；就是一根从模块内部通向外部的信号线。数据类型不指明的话默认为wire
- reg是寄存器。
- 位宽：[n1：n2]，n1比n2大，对应信号从n1到n2，位宽为n1-n2+1

## 5. assign语句（连续赋值语句）
![alt text](image-9.png)
注意：一般都用=来赋值，时序逻辑要用<=来进行赋值。例如上述的D触发器的例子
![alt text](image-10.png)

不同的操作功能：
1. 算数型
![alt text](image-11.png)
注意：求余数时操作数只能是整数且结果符号跟随第一个操作数
2. 逻辑型
![alt text](image-12.png)
3. 关系运算符
![alt text](image-13.png)
注意：1'b1 是 Verilog 里表示常量的一种标准写法。
它的格式是：
位宽 ' 进制 数值
4. 等价运算符
![alt text](image-14.png)
5. 按位运算符
![alt text](image-15.png)
6. 缩减运算符
![alt text](image-16.png)
7. 移位运算符
![alt text](image-17.png)
8. 拼接复制运算符
![alt text](image-18.png)
9. 条件运算符
![alt text](image-19.png)

## 6. always语句块（过程赋值语句）
![alt text](image-20.png)
注意时序赋值要用非阻塞赋值符号<=
1. 敏感条件由敏感信号条件表决定，当敏感条件满足时，过程快被激活。敏感条件有两种：一种是边沿敏感。一种是电平敏感
![alt text](image-21.png)
2. assign和always区别
![alt text](image-22.png)
注意1：always中多条赋值语句用begin--end，赋值是同时进行的，多条并行

注意2：只需要根据“当前输入”得到输出 → 可以考虑 assign。
需要记住“之前的状态” → 通常需要 always @(posedge clk) + FF。
例如下面这个加法计数器的例子：
![alt text](image-24.png)
3. 阻塞赋值和非阻塞赋值
![alt text](image-23.png)

## 7. 底层模块和门原语模块的调用
实际上就是把一个现成的模块与其他模块连接时的描述语句

1. 底层模块调用
格式：底层模块名 例化名（端口映射）
![alt text](image-25.png)
![alt text](image-26.png)
DFF dff2（.CLK(q1)，Q（q），D(d)）最好只用这种命名法，顺序法不好
2. 门原语模块的调用（了解即可）
并列于assign、always的一种赋值方式
![alt text](image-27.png)
![alt text](image-29.png)
![alt text](image-28.png)

## 8.Verilog中的数据类型
- 线网（net）类--wire
不需要保存，用于连续赋值的复制目标或门原语的输出，且仿真时不需要分配内存空间
![alt text](image-30.png)
- 变量（variable）类--reg
需要保存，用于过程赋值的复制目标，且仿真时需要分配内存空间
![alt text](image-31.png)

![alt text](image-32.png)
- input和inout信号必须是net
- output看赋值，assign用net，always用variable
![alt text](image-33.png)

## 9.数字的表示格式
![alt text](image-34.png)
16-h，10-d，8-o，2-b

注意有符号数使用补码表示的哦

## 10.逻辑值
- 1：逻辑1，高电平，数字1
- 0：逻辑0，低电平，数字0
- x：不确定
- z：高阻态

## 11.if语句
![alt text](image-36.png)
## 12.case语句
![alt text](image-35.png)
## 13.Verilog三种描述方式
- 结构化描述：门级描述，全部用门原语和底层模块调用（与或非门构成的逻辑电路图）
![alt text](image-37.png)
- 数据流级描述：全部用assign语句（看逻辑表达式）
![alt text](image-38.png)
- 行为级描述：全部用always语句配合if、case
![alt text](image-39.png)
or可以用“，”表示

实际描述时是三种混合的
- RTL级描述：数据流级+行为级（可综合）

## 14. 其他规定
1. //、/* */为注释
2. Quartus ll要求.v文件名和模块名要一致