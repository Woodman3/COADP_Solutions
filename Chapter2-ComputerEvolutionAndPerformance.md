# 第2章 计算机的演变和性能

## 思考题

1. 什么是存储程序式计算机？
    > 在存储程序式计算机中，程序以某种形式与数据一同存在于存储器中。计算机通过在存储器中读取程序来获取指令，而且通过设置一部分存储器的值就可以编写和修改程序。

1. 任何通用计算机的四个主要部件是什么？
    > **主存储器**：用于存储数据和指令；能够处理二进制数据的**算术逻辑运算单元（ALU）**；**控制器**，负责解释内存中的指令并执行之；由控制器操纵的**输入输出设备（I/O）**。

1. 对集成电路级别而言，计算机系统的3个基本组成部分是什么？
    > 门、存储器位元（Memory cells）和它们之间的互联。

1. 解释摩尔定律。
    > 摩尔观察到单芯片上所能包含的晶体管数量每年翻一番，并正确断言这种态势在不远的将来还会继续下去。

1. 列出并说明计算机系列的主要特性。
    >- **相似或相同的指令集**：多数情况下，同一系列机器中的所有成员都具有完全相同的机器指令级。这样，能够在一台机器上执行的程序同样也能在另一台机器上执行。
    >- **相似或相同的操作系统**：同一系列机器中的所有成员都有相同的基本操作系统。
    >- **更高的速度**：同一系列机器中，从低端到高端成员，其指令执行速度逐渐增加。
    >- **更多的I/O端口数**：同一系列机器中，从低端到高端成员，其I/O端口数逐渐增加。
    >- **更大的内存容量**：同一系列机器中，从低端到高端成员，其内存容量逐渐增加。
    >- **更高的成本**：同一系列机器中，从低端到高端成员，其成本逐渐增加。

1. 区分微处理器的关键特征是什么？
    > 对于微处理器来说，CPU的所有单元都放在同一块芯片内。

---

## 习题

1. 假设**A**=A（1），A（2），…，A（1000）和**B**=B（1），B（2），…，B（1000）是两个向量（一维数组），每个向量包含1000个数，将它们加起来形成数组C，当I=1，2，…，1000，有C（I）=A（I）+B（I）。试用IAS指令集编写一个程序来解决这个问题。忽略IAS只有1000个存储器字单元的事实。
    > 向量A、B和C分别存储在长度为1000的连续存储空间中，它们的起始位置依次为1001、2001和3001，程序开始于第3字的左半字（即3L）。计数变量N被初始化为999，并在每一循环周期结束后将自减1，直到其达到-1时跳出循环。因此，向量加法将由高下标向低下标处理。

    | 位置 | 指令 | 注释 |
    |  --- | --- | ---  |
    | 0| 999 | 定义常数（计数变量N） |
    | 1| 1 | 定义常数 |
    | 2| 1000| 定义常数 |
    | 3L| LOAD M(2000) | 将A（I）送入累加器（AC） |
    | 3R| ADD M(3000) | 计算A（I）+B（I） |
    | 4L| STOR M(4000) | 将和值送入C（I） |
    | 4R| LOAD M(0) | 读入计数变量N |
    | 5L| SUB M(1) | N自减1 |
    | 5R| JUMP +M(6,20:39) | 如果N非负，跳转6R |
    | 6L| JUMP M(6,0:19) | 终止程序 |
    | 6R| STOR M(0) | 更新N |
    | 7L| ADD M(1) | 累加器（AC）自增1 |
    | 7R| ADD M(2) |  |
    | 8L| STOR M(3,8:19) | 更新3L中的地址 |
    | 8R| ADD M(2) |  |
    | 9L| STOR M(3,28:39) | 更新3R中的地址 |
    | 9R| ADD M(2) |  |
    | 10L| STOR M(4,8:19) | 更新4L中的地址 |
    | 10R| JUMP M(3,0:19) | 跳转到3L |
    
1.  1. 在IAS机上，取存储器地址2的内容的机器代码指令应是怎样的？
    1. 为了完成这条指令，在指令周期内CPU需要访问多少次存储器？
    
    | 操作码 | 操作数 |
    | :---: | :---: |
    | 00000001 | 000000000010 |
    > 首先, CPU必须进入存储器中取得指令。指令包含我们将要装载的数据的地址。然后，CPU再次进入存储器去装载位于该地址的数据的值。在整个执行阶段，共需访问两次存储器。

1. 在IAS机上，需要通过将什么放入MAR、MBR、地址总线、数据总线和控制总线，CPU才能完成由存储器读取一个值或向存储器写一个值？请用英语描述此过程。
    > To read a value from memory, the CPU puts the address of the value it wants into the MAR. The CPU then asserts the Read control line to memory and place the address on the address bus. Memory places the contents of the memory location passed on the data bus. This data is then transferred to the MBR. To write a value to memory, the CPU puts the address of the value it wants to write into the MAR. The CPU also places the data it wants to write into the MBR. The CPU then asserts the Write control line to memory and palce the address on the address bus and the data on the data bus. Memory transfers the data on the data bus into the corresponding memory location.

    > 要从存储器上读取一个值，CPU要将该值的地址置入存储器地址寄存器（MAR），然后CPU使存储器读控制线有效（assert）并将地址放置在地址总线上，存储器将存储器上相应位置的内容放置在数据总线上，继而数据被被传送给存储器缓冲寄存器（MBR）。要向存储器写入一个值，CPU要将该值的地址置入存储器地址寄存器（MAR），同时CPU将计划写入的数据置入存储器缓冲寄存器，然后CPU使存储器写控制线有效（assert）并将地址放置在地址总线上，响应数据放置在数据总线上，存储器从数据总线接收数据并写入相应的存储空间。

1. 给出IAS机的存储内容如下,试写出从地址08A开始的该程序的汇编语言代码，并说明这段程序做什么。
   
    | 地址 | 内容 |
    |:---:|:---:|
    | 08A | 010FA210FB |
    | 08B | 010FA0F08D |
    | 08C | 020FA210FB |
    
    > 这段程序将求出位于存储器0FA位置的值的绝对值，将其存储于0FB。
    
    | 地址 | 内容 |
    |:-:|:-:|
    | 08AL | LOAD M(0FA) |
    | 08AR | STOR M(0FA) |
    | 08BL | LOAD M(0FA) |
    | 08BR | JUMP +M(08D) |
    | 08CL | LOAD -M(0FA) |
    | 08CR | STOR M(0FB) |
    | 08D | - |

1. 指出原书图2-3中每条数据路径（例如，AC和ALU之间）的位宽度。
    > 所有通向/来自MBR的数据路径位宽40 bits；所有通向/来自MAR的数据路径位宽12 bits；通向/来自AC的路径位宽40 bits；通向/来自MQ的路径位宽40 bits.

1. 在IBM 360的Model 65和Model 75中，地址在两个分开的主存储器中交错排放（例如，所有的奇数序号字存放在一个存储器中，而所有的偶数字存放在另一个存储器中），采用这一技术的目的是什么？
    > 目的是为了提升性能。当地址呈递给存储器模块时，在读或写操作生效之前会有一定的时延。当上一块存储器模块产生时延时，可以切换至下一地址，指向另一块存储器模块。对于一系列的连续字请求，当存储器模块为奇偶搭配时速率最大。

1. 参照原书表2-4，可以看出IBM 360 Model 75的相对性能是360 Model 30的50倍，而指令周期时间只快了五倍。你如何解释这种差异？
    > 产生这种差异的原因是，除了时钟速率以外，系统的其他组件也会对也会对系统整体的运行速率产生较大的影响。尤其是提升存储系统和I/O处理的性能有助于提升系统的总体性能。由于木桶效应，系统整体的速度只能达到系统中各模块间连接速度的最小值。近几年，性能瓶颈已经转移至存储模块和总线速率上。

1. 逛比利·鲍勃的计算机商店时，你听到一个顾客问比利·鲍勃他在该商店能买到的最快的计算机是什么。比利·鲍勃回答说：“你正在看的是我们的Macintosh机器，最快的Mac机以1.2GHz的时钟速度运行。如果你实在想要最快的机器，你应该购买我们的2.4GHz的Intel Pentium 4计算机。”比利·鲍勃的回答对吗？你应该说些什么来帮助这位顾客？
    > 由第7题可知，虽然Intel的机器拥有更高的时钟频率（2.4GHz vs 1.2GHz），但是并不足以说明整体系统的性能将会更好。在不同的系统之间，单纯的时钟频率不具有可比性。像系统组成（存储器、总线、结构）和指令集等其他因素通常也必须考虑在内。更精准的测量方法是在双方系统上运行基准程序（Benchmark）。基准程序执行诸如办公软件、浮点处理性能、图像处理等确定的测试。通过二者完成所有测试所消耗的时间来进行比较。在苹果电脑中，对于很多基准程序，G4的性能不弱甚至优于具有更高时钟频率的Pentium计算机。

1. ENIAC是一个十进制机器，它用十个电子管绕成一个环来表示一个寄存器。在任何时刻，只有一个电子管处于ON状态，表示10个数字中的一个。假定，ENIAC有能力使多个电子管同时处于ON和OFF状态，为什么这种表示法是一种“浪费”，我们用十个电子管所能表示整数范围是什么？
    > 之所以说这种表示法是一种“浪费”，是因为为了表示0到9这一位的十进制数需要十个电子管。如果我们有能力让多个电子管同时处于ON的状态，那么这些电子管就可以被当作二进制位来看待。当有十位时，我们可以表示1024种不同的形式，也就是说我们可以表示整数0至1023。

1. 某基准程序在一个40MHz的处理器上运行，其目标代码有100 000条指令，由如下各指令及时钟周期技术混合组成，试计算该程序的有效CPI、MIPS速率和执行时间。

    | 指令类型 | 指令条数 | 执行每条指令的周期数 |
    |:-:|:-:|:-:|
    | 整数运算 | 45000 | 1 |
    | 数据传送 | 32000 | 2 |
    | 浮点数运算 | 15000 | 2 |
    | 控制传送 | 8000 | 2 |

    > CPI=1.55；MIPS速率=25.8；执行时间=1.87ns。

1. 考虑两类不同的机器，具有两种不同的指令集，两者的时钟频率都是200MHz。在此两种计算机上运行一组给定的基准程序的结果如下，
    |指令类型|指令条数（百万）|执行每条指令的周期数|
    |:-:|:-:|:-:|
    | 机器A | --- | --- |
    | 算数和逻辑运算 | 8 | 1 |
    | 取数和存数 | 4 | 3 |
    | 分支 | 2 | 4 |
    | 其他 | 4 | 3 |
    | --- | --- | --- |
    | 机器B | --- | --- |
    | 算数和逻辑运算 | 10 | 1 |
    | 取数和存数 | 8 | 2 |
    | 分支 | 2 | 4 |
    | 其他 | 4 | 3 |
    1. 试计算每台机器的有效CPI、MIPS速率及执行时间。
    2. 评论结果。 
    >1. <img src="https://latex.codecogs.com/png.latex?CPI_A=\frac{\sum&space;CPI_i&space;\times&space;I_i}{I_c}=\frac{(8&space;\times&space;1&space;&plus;&space;4&space;\times&space;3&space;&plus;&space;2\times4&plus;4\times3)\times10^6}{(8&plus;4&plus;2&plus;4)\times10^6}\approx&space;2.22" title="CPI_A=\frac{\sum CPI_i \times I_i}{I_c}=\frac{(8 \times 1 + 4 \times 3 + 2\times4+4\times3)\times10^6}{(8+4+2+4)\times10^6}\approx 2.22" /> </n>
    <img src="https://latex.codecogs.com/png.latex?MIPS_A=\frac{f}{CPI_A\times&space;10^6}=\frac{200\times10^6}{2.22\times10^6}=90" title="MIPS_A=\frac{f}{CPI_A\times 10^6}=\frac{200\times10^6}{2.22\times10^6}=90" /></n><img src="https://latex.codecogs.com/png.latex?CPU_A=\frac{I_c\times&space;CPI_A}{f}=\frac{18\times10^6\times2.2}{200\times10^6}=0.2s" title="CPU_A=\frac{I_c\times CPI_A}{f}=\frac{18\times10^6\times2.2}{200\times10^6}=0.2s" /> </n><img src="https://latex.codecogs.com/png.latex?CPI_B=\frac{\sum&space;CPI_i&space;\times&space;I_i}{I_c}=\frac{(10&space;\times&space;1&space;&plus;&space;8&space;\times&space;2&space;&plus;&space;2\times4&plus;4\times3)\times10^6}{(10&plus;8&plus;2&plus;4)\times10^6}\approx&space;1.92" title="CPI_B=\frac{\sum CPI_i \times I_i}{I_c}=\frac{(10 \times 1 + 8 \times 2 + 2\times4+4\times3)\times10^6}{(10+8+2+4)\times10^6}\approx 1.92" /><n/><img src="https://latex.codecogs.com/png.latex?MIPS_B=\frac{f}{CPI_B\times&space;10^6}=\frac{200\times10^6}{1.92\times10^6}=104" title="MIPS_B=\frac{f}{CPI_B\times 10^6}=\frac{200\times10^6}{1.92\times10^6}=104" /></n><img src="https://latex.codecogs.com/png.latex?CPU_A=\frac{I_c\times&space;CPI_B}{f}=\frac{24\times10^6\times1.92}{200\times10^6}=0.23s" title="CPU_A=\frac{I_c\times CPI_B}{f}=\frac{24\times10^6\times1.92}{200\times10^6}=0.23s" />
    >1. 尽管机器B比机器A有更高的MIPS，但它却要花费更长的时间去执行基准程序。

1. CSIC和RSIC设计的早期例子分别是VAX 11/780和IBM RS/600.使用一个典型的基准程序，产生如下的机器特征结果：

    | 处理器 | 时钟频率 | 性能 | CPU时间 |
    |:-:|:-:|:-:|:-:|
    | VAX 11/780 | 5 MHz| 1 MIPS | 12x 秒 |
    | IBM RS/6000 | 25 MHz | 18 MIPS | x 秒 |
    最后一列显示VAX需要的CPU时间是IBM机器的12倍。
    1. 运行在这两台机器上的基准程序的机器代码的指令条数的关系是什么?
    1. 这两台机器的CPI各是多少？
    > 1. 通过式<img src="https://latex.codecogs.com/png.latex?\frac{MIPS}{10^6}=\frac{I_c}{T}" title="\frac{MIPS}{10^6}=\frac{I_c}{T}" />可得<img src="https://latex.codecogs.com/png.latex?I_c=\frac{MIPS\times&space;T}{10^6}" title="I_c=\frac{MIPS\times T}{10^6}" />。故RS/6000与VAX指令数之比为<img src="https://latex.codecogs.com/png.latex?\frac{x\times18}{12x\times1}=1.5" title="\frac{x\times18}{12x\times1}=1.5" />。
    > 1. 对于VAX，<img src="https://latex.codecogs.com/png.latex?CPI=\frac{5&space;MHz}{1&space;MIPS}=5" title="CPI=\frac{5 MHz}{1 MIPS}=5" />。对于RS/6000，<img src="https://latex.codecogs.com/png.latex?CPI=\frac{25&space;MHz}{18&space;MIPS}=1.39" title="CPI=\frac{25 MHz}{18 MIPS}=1.39" />。

1. 在三台计算机上运行四个基准程序的结果如下：
    |  | 计算机A | 计算机B | 计算机C |
    |-:|-:|-:|-:|
    | 程序1 | 1 | 10 | 20 |
    | 程序2 | 1000 | 100 | 20 |
    | 程序3 | 500 | 1000 | 50 |
    | 程序4 | 100 | 800 | 100 |
    上表显示以秒为单位在各机器上运行1亿条指令的执行时间，试计算每台计算机运行每种程序的MIPS值。假设这四个程序的权重相同，试计算其算数平均值和调和平均值，并按算术平均值和调和平均值排列该三台计算机。
    > 由式<img src="https://latex.codecogs.com/png.latex?MIPS=\frac{I_c}{T\times10^6}=\frac{100}{T}" title="MIPS=\frac{I_c}{T\times10^6}=\frac{100}{T}" />得MIPS值于下表：
    
    |  | 计算机A | 计算机B | 计算机C |
    |:-:|:-:|:-:|:-:|:-:|
    | 程序1 | 100 | 10 | 5 |
    | 程序2 | 0.1 | 1 | 5 |
    | 程序3 | 0.2 | 0.1 | 2 |
    | 程序4 | 1 | 0.125 | 1 |
    > 排序如下表：
    
    |   |算术平均值|排序|几何平均值|排序|
    |:-:|:-:|:-:|:-:|:-:|:-:|
    |计算机A|25.325|1|0.25|2|
    |计算机B|2.8|3|0.21|3|
    |计算机C|3.26|2|2.1|1|

1. 下表显示在三台机器上运行五种不同基准程序的执行时间，以秒为单位。
    |基准程序||处理器||
    |:-:|:-:|:-:|:-:|
    |---|R|M|Z|
    |E|417|244|134|
    |F|83|70|70|
    |H|66|153|135|
    |I|39449|35527|66000|
    |K|772|368|369|
    1. 首先，正规化（Normalized）到机器R，计算每台计算机运行每种基准程序的速度度量，也就是说，让R的速度值都为1.0，将机器R作为参照系统，使用原书公式（2.5）计算其他机器的速度，是用原书公式（2.3）计算每个系统的算术平均值。
    1. 采用机器M作为参照系统重做问题1。
    1. 基于前面两种计算之一，指出哪一台机器是最慢的。
    1. 使用几何平均值的计算公式（2.6），重做问题1和2的计算，并基于这两种计算指出哪一台机器是最慢的。
    >1. 对R归一化（Normalized）：
    
    |基准程序||处理器||
    |:-:|:-:|:-:|:-:|
    |---|R|M|Z|
    |E|1.00|1.71|3.11|
    |F|1.00|1.19|1.19|
    |H|1.00|0.43|0.49|
    |I|1.00|1.11|0.60|
    |K|1.00|2.10|2.09|
    | 算数平均值 |1.00|1.31|1.50|
    >2. 对M归一化（Normalized）：
    
    |基准程序||处理器||
    |:-:|:-:|:-:|:-:|
    |---|R|M|Z|
    |E|0.59|1.00|1.82|
    |F|0.84|1.00|1.00|
    |H|2.32|1.00|1.13|
    |I|0.90|1.00|0.54|
    |K|0.48|1.00|1.00|
    | 算数平均值 |1.01|1.00|1.10|
    >3. 比值越大速度越快。基于第1问，在大量数据测试时，R是速度最慢的机器；基于第二问，在少量数据测试时，M是速度最慢的机器。
    
    >4. 对R归一化（Normalized）：
    
    |基准程序||处理器||
    |:-:|:-:|:-:|:-:|
    |---|R|M|Z|
    |E|1.00|1.71|3.11|
    |F|1.00|1.19|1.19|
    |H|1.00|0.43|0.49|
    |I|1.00|1.11|0.60|
    |K|1.00|2.10|2.09|
    | 几何平均值 |1.00|1.15|1.18|
    
    |基准程序||处理器||
    |:-:|:-:|:-:|:-:|
    |---|R|M|Z|
    |E|0.59|1.00|1.82|
    |F|0.84|1.00|1.00|
    |H|2.32|1.00|1.13|
    |I|0.90|1.00|0.54|
    |K|0.48|1.00|1.00|
    | 几何平均值 |0.87|1.00|1.02|
    > 基于几何平均值，无论对谁做归一化，R都是速度最慢的机器。

1. 为了分清前面问题的结果，考虑一个更简单的例子。

    | 基准程序 | | 处理器 | |
    |:-:|:-:|:-:|:-:|:-:|
    | --- | X | Y | Z |
    | 1 | 20 | 10 | 40 |
    | 2 | 40 | 80 | 20 |
    1. 首先将X作为参照机器，然后将Y作为参照机器，分别为每个系统计算算术平均值。辩论直观上该三台机器由大致相等的性能以及算术平均值给出了误导性的结果。
    1. 首先将X作为参照机器，然后将Y作为参照机器，分别计算每个系统的几何平均值。说明这些比算术平均更现实。
    > 1. 归一化X
    
    |基准程序||处理器||
    |:-:|:-:|:-:|:-:|
    |---|X|Y|Z|
    |1|1.00|2.00|0.50|
    |2|1.00|0.50|2.00|
    | 算术平均值 |1.00|1.25|1.25|
    | 几何平均值 |1.00|1|1|
    > 归一化Y
    
    |基准程序||处理器||
    |:-:|:-:|:-:|:-:|
    |---|X|Y|Z|
    |1|0.50|1.00|0.25|
    |2|2.00|1.00|4.00|
    | 算术平均值 |1.25|1.00|2.13|
    | 几何平均值 |1.00|1.00|1.00|
    > 机器Y在基准程序1中有两倍于机器X的性能，但在基准程序2中只有机器X的一半性能。同样的，机器Z在基准程序1中只有X一半的性能，但在基准程序2中有两倍性能。直观上看，这三台机器应拥有相同的性能，然而，但我们对X进行归一化并计算速度量化后的的算术平均值，我们会发现Y与Z会比X快25%。现在，如果我们对Y归一化并计算量化后的算术平均值，我们会发现X比Y快25%而Z比Y快两倍。显然算术平均值在该比较中是价值义的。
    > 2. 当使用几何平均值时，对X进行归一化，可以发现三台机器显示出它们具有相同的性能表现。归一化Y也可以得到相同的结论。该结果更加符合我们的直观感受。

1. 考虑2.5节中计算平均CPI和MIPS速度的例子，该例子得到的结果是CPI=2.24和MIPS速度=178.现假设该程序可以以8个并行的任务或线程的方式执行，而每个任务的指令条数大致相等。该程序在一个8核的系统上执行，每个核（处理器）的性能与最初使用的单核的性能相同。各部分之间的协调和同步使每个任务增加额外的25 000条指令的执行。假设每个任务的指令混合与例子中相同，但由于对存储器的竞争，使cache失效时存储器访问的CPI增大到12个周期。
    1. 计算平均CPI。
    1. 计算相应的MIPS速度。
    1. 计算加速比因子。
    1. 将实际加速比因子与由阿姆达尔定律决定的理论加速比因子进行比较。
    > 1. 假设每个任务的额外分配的指令类型的比例与原指令类型比例相同，我们可以得到如下表格：
    
    |指令类型|CPI|指令比例|
    |:-:|:-:|:-:|:-:|
    |算术和逻辑运算|1|60%|
    |cache命中的取数和存数|2|18%|
    |分支|4|12%|
    |cache未命中的存储器查询|12|10%|
    > <img src="https://latex.codecogs.com/png.latex?CPI=0.6&plus;(2\times0.18)&plus;(4\times0.12)&plus;(12\times0.1)=2.64" title="CPI=0.6+(2\times0.18)+(4\times0.12)+(12\times0.1)=2.64" />。CPI随着存储器存取时间的增长而增长。
    
    >2. <img src="https://latex.codecogs.com/png.latex?MIPS=\frac{400}{2.64}=152" title="MIPS=\frac{400}{2.64}=152" /> 。MIPS速度也有相应的下降。
    >1.  运行时间的比例是加速的因素。由公式（2.2），我们可以计算出执行时间<img src="https://latex.codecogs.com/png.latex?T=\frac{I_c}{MIPS\times10^6}" title="T=\frac{I_c}{MIPS\times10^6}" />.考虑单处理器情况，<img src="https://latex.codecogs.com/png.latex?T_1=\frac{2\times10^6}{178\times10^6}=11ms" title="T_1=\frac{2\times10^6}{178\times10^6}=11ms" />。当为8处理器时，每个处理器执行二百万原始指令的1/8加上25 000新增指令。此时在8处理器中每个处理器的执行时间为<img src="https://latex.codecogs.com/png.latex?T_8=\frac{\frac{2\times10^6}{8}&plus;0.025\times10^6}{152\times10^6}=1.8ms" title="T_8=\frac{\frac{2\times10^6}{8}+0.025\times10^6}{152\times10^6}=1.8ms" />。因此我们得到*加速比=11/1.8=6.11*。
    >1. 这个问题的答案依赖于我们如何理解阿姆达尔定律。并行系统存在两种可能降低效率的情况。第一，为了协调不同线程而添加额外的指令。第二，对存储器存取的争用。通常来说，没有代码是天生就是串行执行的。所有代码都是可以并行执行的,但会提前对执行顺序进行规划。有人认为存储器存取与这是矛盾的，也就是从某方面来说，存储器查询指令是不可并行执行的。但是依据所给出的信息，我们并不清楚该如何在阿姆达尔公式中量化这一影响。如果我们假设代码中可以并行执行的那一部分为*f=1*，那么在这种情况下阿姆达尔定律将简化为*加速比=N=8*，因此实际的加速比只相当于理论加速比的75%。

1. 一个处理器访问主存的平均访问时间为<img src="https://latex.codecogs.com/png.latex?T_2" title="T_2" />。一个容量比较小的cache存储器插在处理器与主存之间。cache的访问速度比主存快很多，即<img src="https://latex.codecogs.com/png.latex?T_1<T_2" title="T_1<T_2" />。任何时候，cache保存主存的一部份拷贝，以便在不久的将来CPU最可能访问的字在cache中。假设处理器访问的下一个字在cache中的可能性为*H*，即命中率为*H*。
    1. 对于任何单个存储器访问，在cache中访问该字与在主存中访问该字的理论加速比是多少？
    2. 假设*T*为平均访问时间，将*T*表示为<img src="https://latex.codecogs.com/png.latex?T_1" title="T_1" />，<img src="https://latex.codecogs.com/png.latex?T_2" title="T_2" />和*H*的函数，总加速比与*H*的函数是什么？
    1. 实际上，系统被设计为处理器必须首先访问cache，并决定该字是否在cache中，如果该字不在cache中，然后才访问内存。因此，当cache访问失效（与“命中”相反）时，存储器的访问时间是<img src="https://latex.codecogs.com/png.latex?T_1+T_2" title="T_1+T_2" />。将*T*表示为<img src="https://latex.codecogs.com/png.latex?T_1" title="T_1" />，<img src="https://latex.codecogs.com/png.latex?T_2" title="T_2" />和*H*的函数。现计算其加速比，并与前一问中的结果进行比较。
    > 1. <img src="https://latex.codecogs.com/png.latex?Speedup=\frac{T_2}{T_1}" title="Speedup=\frac{T_2}{T_1}" />
    > 1.  可计算出平均存取时间为<img src="https://latex.codecogs.com/png.latex?T=H\times&space;T_1&plus;(1-H)\times&space;T_2" title="T=H\times T_1+(1-H)\times T_2" />。由公式（2.8）得<img src="https://latex.codecogs.com/png.latex?Speedup=\frac{T_2}{T}=\frac{T_2}{H\times&space;T_1&plus;(1-H)\times&space;T_2}=\frac{1}{(1-H)&plus;H\frac{T_1}{T_2}}" title="Speedup=\frac{T_2}{T}=\frac{T_2}{H\times T_1+(1-H)\times T_2}=\frac{1}{(1-H)+H\frac{T_1}{T_2}}" />
    >1. <img src="https://latex.codecogs.com/png.latex?T=H\times&space;T_1&space;&plus;&space;(1-H)\times&space;(T_1&plus;T_2)=T_1&plus;(1-H)\times&space;T_2" title="T=H\times T_1 + (1-H)\times (T_1+T_2)=T_1+(1-H)\times T_2" />，此式为公式（4.2）。此时，<img src="https://latex.codecogs.com/png.latex?Speedup=\frac{T_2}{T}=\frac{T_2}{T_1&plus;(1-H)\times&space;T_2}=\frac{1}{(1-H)&plus;\frac{T_1}{T_2}}" title="Speedup=\frac{T_2}{T}=\frac{T_2}{T_1+(1-H)\times T_2}=\frac{1}{(1-H)+\frac{T_1}{T_2}}" />。当前情况，分母较之前更大，故加速比更小。