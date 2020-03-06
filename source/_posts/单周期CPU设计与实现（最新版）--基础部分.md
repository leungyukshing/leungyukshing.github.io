---
title: 单周期CPU设计与实现（最新版）--基础部分
tags:
  - Verilog
abbrlink: 42958
date: 2018-08-07 23:41:59
---
## 概述
&emsp;&emsp;计组课程的其中一个大实验--实现一个单周期CPU。所谓单周期，是指**一条指令在一个时钟周期内执行完**，这是单周期CPU的核心思想。接下来我将详细介绍整个实验的内容与实现，代码可以直接看https://github.com/leungyukshing/-CPU-。
<!-- more -->

## CPU的工作阶段
&emsp;&emsp;CPU在处理每一条指令的时候，一般会经过以下几个步骤（有的指令不是所有的都需要执行，这也是单周期与多周期的区别）：
  + **取指（IF）**：根据程序计数器PC中的指令地址，从指令存储器中取出一条指令，同时，PC根据指令字长度自动递增产生下一条指令所需要的指令地址（一般是+4），但遇到“地址转移”指令时，则控制器把“转移地址”送入PC，当然得到的“地址”需要做些变换才送入PC。
  + **译码（ID）**：对取指令操作中得到的指令进行分析并译码，确定这条指令需要完成的操作，从而产生相应的操作码，用于驱动执行状态中的各种操作。
  + **执行（EXE）**：根据指令译码得到的操作控制信号，使用ALU具体地执行指令动作，然后转移到结果写回状态。
  + **访存（MEM）**：所有需要访问存储器的操作都将在这个步骤中执行，该步骤给出存储器的数据地址，把数据写入到存储器中数据地址所指定的存储单元或者从存储器中得到数据地址单元中的数据。
  + **写回（WB）**：指令执行的结果或者访问存储器中得到的数据写回相应的目的寄存器中。

## 数据通路
&emsp;&emsp;一条指令的执行需要不同部件的配合，如何使用这些部件就需要控制单元发出控制信号。指令以及数据在CPU各部件中的移动是使用数据通路图来描述的，老师提供的数据通路图如下：
![数据通路图](/images/cpu_data_path.png)
&emsp;&emsp;接下来我们在实现的过程中，需要紧密地联系以上这幅数据通路图，因此最好先熟悉这幅图，包括每个模块的接口，每个数据的位数等。
&emsp;&emsp;每条指令由不同模块功能相互组合而成，模块主要有程序计数器（PC），指令寄存器（rom），寄存器组（register），控制单元（control unit），ALU运算器（ALU），数据存储器（ram），符号或者零拓展模块（extend）以及加法器，移位器和几个二选一数据选择器。

## 代码实现
&emsp;&emsp;在vivado中使用verilog语言逐个`.v`文件实现，以下是主要模块的代码。

### 1.控制单元（ControlUnit.v）
&emsp;&emsp;这里需要根据每条指令所用到的模块，通过建立对应的真值表，对每个控制信号进行赋值，以确定使用哪些模块。
&emsp;&emsp;确定控制信号真值表，也是整个CPU逻辑最重要的部分。
![控制信号真值表](/images/cpu_signal_truth_table.png)

```verilog
module ControlUnit(
    input[5:0] Opcode,
    input zero,
    input sign,
    output RegDst,
    output InsMemRW,
    output PCWre,
    output ExtSel,
    output DBDataSrc,
    output mWR,
    output mRD,
    output ALUSrcA,
    output ALUSrcB,
    output [1:0] PCSrc,
    output [2:0] ALUOp,
    output RegWre
    );

    // Control Signals
    assign RegDst = (Opcode == 6'b000000 || Opcode == 6'b000010 || Opcode == 6'b010001 || Opcode == 6'b010010 || Opcode == 6'b011000) ? 1 : 0;
    assign PCWre = (Opcode == 6'b111111) ? 0 : 1;
    assign ExtSel = (Opcode == 6'b010000) ? 0 : 1;
    assign DBDataSrc = (Opcode == 6'b100111) ? 1 : 0;
    assign mWR = (Opcode == 6'b100110) ? 1 : 0;
    assign mRD = (Opcode == 6'b100111) ? 1 : 0;
    assign ALUSrcA = (Opcode == 6'b011000) ? 1 : 0;
    assign ALUSrcB = (Opcode == 6'b000001 || Opcode == 6'b010000 || Opcode == 6'b011011 || Opcode == 6'b100110 || Opcode == 6'b100111) ? 1 : 0;
    assign RegWre = (Opcode == 6'b100110 || Opcode == 6'b110000 || Opcode == 6'b110001 || Opcode == 6'b111000 || Opcode == 6'b111111) ? 0 : 1;

    // PCSrc - choose next address
    assign PCSrc[0] = ((Opcode == 6'b110000 && zero == 1) || (Opcode == 6'b110001 && zero == 0)) ? 1 : 0;
    assign PCSrc[1] = (Opcode == 6'b111000) ? 1 : 0;

    // ALUOp - choose ALU functions
    assign ALUOp[0] = (Opcode == 6'b000010 || Opcode == 6'b010010 || Opcode == 6'b010000 || Opcode == 6'b110000 || Opcode == 6'b110001) ? 1 : 0;
    assign ALUOp[1] = (Opcode == 6'b010010 || Opcode == 6'b010000 || Opcode == 6'b011000 || Opcode == 6'b011011) ? 1 : 0;
    assign ALUOp[2] = (Opcode == 6'b010001 || Opcode == 6'b011011) ? 1 : 0;

endmodule
```

### 2.程序计数器（PC.v）
&emsp;&emsp;为了分离出PC和PC+4，这里特意分成两个模块。PC模块是设置PC为PCnext。在**时钟上升沿**到来或**清零信号**到来的时候，设置新的PC。

```verilog
module PC(
    input PCWre,
    input clk,
    input Reset,
    input[31:0] pcin,
    output[31:0] ExtOut,
    output[31:0] pc
    );
    reg[31:0] pc;

    always@(posedge clk or negedge Reset)
        begin
            if (!Reset)
                pc = 0;
            else
                begin
                    if (PCWre)
                        begin
                            pc = pcin;
                        end
                end
        end
endmodule
```

### 3.PC选择器（Mux.v）
&emsp;&emsp;根据指令形式，确定下一个PC是PC+4还是跳转。这里牵涉到了数据选择的功能，因此我选择把他分离出来。
```verilog
module Mux(
    input clk,
    input[31:0] pc,
    input[31:0] ExtOut,
    input[25:0] JumpAddr,
    input[1:0] PCSrc,
    input PCWre,
    output reg[31:0] nextpc
    );

    always@(*)
            begin
                if(!PCWre) begin
                    nextpc = pc;
                    end
                else begin
                case(PCSrc)
                    2'b00:
                        begin
                            nextpc = pc + 4;
                        end
                                    2'b01:
                                        begin
                                            nextpc = pc + 4 + ExtOut * 4;
                                        end
                                    2'b10:
                                        begin
                                            nextpc = pc + 4;

                                            nextpc[27:2] = JumpAddr[25:0];
                                            nextpc[1] = 0;
                                            nextpc[0] = 0;
                                        end
                                    2'b11:
                                        begin
                                        end
                                endcase
                                end
            end
endmodule
```

### 4.指令寄存器（InsMEM.v）
&emsp;&emsp;指令寄存器是由寄存器组成的，存储所有我们将要执行的指令。这里采用的是8位大端的存储模式。
&emsp;&emsp;这里先给出我们需要测试的指令序列。
![测试程序段](/images/cpu_test_program_section.png)

```verilog
module InsMEM(
    input[31:0] IAddr,
    input InsMemRW,
    output[5:0] op,
    output[4:0] rs,
    output[4:0] rt,
    output[4:0] rd,
    output[4:0] sa,
    output[15:0] imd,
    output[25:0] JumpAddr
    );
    wire[7:0] ins[0:100];

    // store all instructions

    // addi $1, $0, 8  04010008
    assign ins[0] = 8'h04;
    assign ins[1] = 8'h01;
    assign ins[2] = 8'h00;
    assign ins[3] = 8'h08;

    // ori $2. $0, 2  40020002
    assign ins[4] = 8'h40;
    assign ins[5] = 8'h02;
    assign ins[6] = 8'h00;
    assign ins[7] = 8'h02;

    // add $3, $2, $1  00411800
    assign ins[8] = 8'h00;
    assign ins[9] = 8'h41;
    assign ins[10] = 8'h18;
    assign ins[11] = 8'h00;

    // sub $5, $3, $2  08622800
    assign ins[12] = 8'h08;
    assign ins[13] = 8'h62;
    assign ins[14] = 8'h28;
    assign ins[15] = 8'h00;

    // and $4, $5, $2  44a22000
    assign ins[16] = 8'h44;
    assign ins[17] = 8'ha2;
    assign ins[18] = 8'h20;
    assign ins[19] = 8'h00;

    // or $8, $4, $2  48824000
    assign ins[20] = 8'h48;
    assign ins[21] = 8'h82;
    assign ins[22] = 8'h40;
    assign ins[23] = 8'h00;

    // sll $8, $8, 1  60084040
    assign ins[24] = 8'h60;
    assign ins[25] = 8'h08;
    assign ins[26] = 8'h40;
    assign ins[27] = 8'h40;

    // bne $8, $1, -2  c501fffe
    assign ins[28] = 8'hc5;
    assign ins[29] = 8'h01;
    assign ins[30] = 8'hff;
    assign ins[31] = 8'hfe;

    // slti $6, $2, 8  6c460008
    assign ins[32] = 8'h6c;
    assign ins[33] = 8'h46;
    assign ins[34] = 8'h00;
    assign ins[35] = 8'h08;

    // slti $7, $6, 0  6cc70000
    assign ins[36] = 8'h6c;
    assign ins[37] = 8'hc7;
    assign ins[38] = 8'h00;
    assign ins[39] = 8'h00;

    // addi $7, $7, 8  04e70008
    assign ins[40] = 8'h04;
    assign ins[41] = 8'he7;
    assign ins[42] = 8'h00;
    assign ins[43] = 8'h08;

    // beq $7, $1, -2  c0e1fffe
    assign ins[44] = 8'hc0;
    assign ins[45] = 8'he1;
    assign ins[46] = 8'hff;
    assign ins[47] = 8'hfe;

    // sw $2, 4($1)  98220004
    assign ins[48] = 8'h98;
    assign ins[49] = 8'h22;
    assign ins[50] = 8'h00;
    assign ins[51] = 8'h04;

    // lw $9, 4($1)  9c290004
    assign ins[52] = 8'h9c;
    assign ins[53] = 8'h29;
    assign ins[54] = 8'h00;
    assign ins[55] = 8'h04;

    // j 0x00000040  e00000010
    assign ins[56] = 8'he0;
    assign ins[57] = 8'h00;
    assign ins[58] = 8'h00;
    assign ins[59] = 8'h10;

    // addi $10, $0, 10  040a000a
    assign ins[60] = 8'h04;
    assign ins[61] = 8'h0a;
    assign ins[62] = 8'h00;
    assign ins[63] = 8'h0a;

    // halt  fc000000
    assign ins[64] = 8'hfc;
    assign ins[65] = 8'h00;
    assign ins[66] = 8'h00;
    assign ins[67] = 8'h00;

    // read instruction
    wire[31:0] temp;
    assign temp[31:24] = ins[IAddr[6:2]*4];
    assign temp[23:16] = ins[IAddr[6:2]*4+1];
    assign temp[15:8] = ins[IAddr[6:2]*4+2];
    assign temp[7:0] = ins[IAddr[6:2]*4+3];

    assign op = temp[31:26];
    assign rs = temp[25:21];
    assign rt = temp[20:16];
    assign rd = temp[15:11];
    assign sa = temp[10:6];
    assign imd = temp[15:0];
    assign JumpAddr = temp[25:0];

endmodule
```

### 5.寄存器组（RegisterFile.v）
&emsp;&emsp;寄存器组是用**32个32位的寄存器**组成的存储模块，用于存储运算中的一些变量，模拟的是CPU中的寄存器。现实中，当CPU断电后，这一部分的内容将会被清空。

```verilog
module RegisterFile(
    input clk,
    input[4:0] rs,
    input[4:0] rt,
    input[4:0] rd,
    input RegDst,
    input[31:0] result,
    input[31:0] DataOut,
    input DBDataSrc,
    input RegWre,
    output[31:0] ReadData1,
    output[31:0] ReadData2
    );

    reg[31:0] registers[31:0];
    integer i;

    // Initialize the registers
    initial
        begin
            for(i = 0; i < 32; i=i+1)
                registers[i] <= 0;
        end

    wire[4:0] regToWrite;
    wire[31:0] WriteData;
    // choose reg
    assign regToWrite = (RegDst ? rd : rt);
    // choose data
    assign WriteData = (DBDataSrc ? DataOut : result);

    // Read
    assign ReadData1 = registers[rs];
    assign ReadData2 = registers[rt];

    // Write
    always @(negedge clk)
        begin
            if (RegWre && regToWrite)
                registers[regToWrite] <= WriteData;
        end
endmodule
```

### 6.算术逻辑运算单元（ALU.v）
&emsp;&emsp;ALU是CPU中的核心部分，用于指令执行中的算术和逻辑运算，返回运算的结果可能是控制跳转的信号，也可能是计算结果，需要写回寄存器或者写入到存储器。

```verilog
module ALU(
    input[31:0] ReadData1,
    input[31:0] ReadData2,
    input[31:0] ExtOut,
    input[4:0] sa,
    input ALUSrcA,
    input ALUSrcB,
    input[2:0] ALUOp,
    output zero,
    output [31:0] result,
    output sign
    );

    reg zero;
    reg[31:0] result;
    wire [31:0] A;
    wire [31:0] B;
    // Operand 1
    assign A = (ALUSrcA ? sa : ReadData1);
    // Operand 2
    assign B = (ALUSrcB ? ExtOut : ReadData2);

    always@(*)
        begin
            case(ALUOp)
                3'b000:
                    begin
                        result <= A + B;
                    end
                3'b001:
                    begin
                        result <= A - B;
                    end
                3'b010:
                    begin
                        result <= B << A;
                    end
                3'b011:
                    begin
                        result <= A | B;
                    end
                3'b100:
                    begin
                        result <= A & B;
                    end
                3'b101:
                    begin
                        result <= (A < B) ? 1 : 0;
                    end
                3'b110:
                    begin
                        result <= (((A < B) && (A[31] == B[31])) || ((A[31] == 1 && B[31] == 0))) ? 1 : 0;
                    end
                3'b111:
                    begin
                        result <= A ^ B;
                    end
                default:
                    result <= 0;
            endcase

            if(!result)
                zero = 1;
            else
                zero = 0;
        end
endmodule
```

### 7.数据存储器（DataMEM.v）
&emsp;&emsp;数据存储器使用8位字长的存储器，同样采用**大端模式**。现实中，这一部分模拟的是现实中的二级存储器--外存。当CPU断电时，这一部分的信息是不会丢失的。

```verilog
module DataMEM(
    input[31:0] DAddr,
    input[31:0] DataIn,
    input mRD,
    input mWR,
    input clk,
    output reg[31:0] DataOut
    );
    initial begin
        DataOut <= 0;
    end
    reg [8:0] memory[31:0];
    integer i;

    initial
        begin
            for(i = 0; i < 32; i=i+1)
                memory[i] <= 0;
        end

    always @(*)
        begin
        // read data
            if (mRD)
                begin
                    DataOut[31:24] = memory[DAddr];
                    DataOut[23:16] = memory[DAddr+1];
                    DataOut[15:8] = memory[DAddr+2];
                    DataOut[7:0] = memory[DAddr+3];
                end
        // write data
            else if (mWR && !clk)
                begin
                    memory[DAddr] <= DataIn[31:24];
                    memory[DAddr+1] <= DataIn[23:16];
                    memory[DAddr+2] <= DataIn[15:8];
                    memory[DAddr+3] <= DataIn[7:0];
                    DataOut <= 0;
                end
        end
endmodule
```

### 8.符号位拓展单元（Extend.v）
&emsp;&emsp;CPU在处理数据的过程中，会遇到一些位数不一样的数据，比如在指令中提取的立即数，需要拓展成32位数据才可以给ALU运算。

```verilog
module Extend(
    input[15:0] imd,
    input ExtSel,
    output [31:0] ExtOut
    );

    assign ExtOut[15:0] = imd;
    assign ExtOut[31:16] = (ExtSel ? (imd[15] ? 16'hffff: 16'h0000) : 16'h0000);
endmodule
```


### 9.顶层模块（s_cpu.v）
&emsp;&emsp;上面实现了CPU中的8大模块，接下来就需要有一个**顶层模块**来连接各模块，运行整个CPU。
&emsp;&emsp;根据数据通路图，连接各模块的接口，**wire变量就相当于连线**，这个是比较好理解的。

```verilog
module s_cpu(
    input clk,
    input reset,
    output[5:0] Opcode,
    output[31:0] Data1,
    output[31:0] Data2,
    output[31:0] pc,
    output[31:0] pcnext,
    output[31:0] result,
    output[4:0] rs,
    output[4:0] rt,
    output[31:0] DataOut
    );
    wire[2:0] ALUOp;
    wire[31:0] ExtOut;
    wire[25:0] JumpAddr;
    wire[15:0] immediate;
    wire[4:0]  rd, sa;
    wire[1:0] PCSrc;
    wire RegDst, zero, sign, PCWre, ALUSrcA, ALUSrcB, ExtSel, DBDataSrc, mWR, mRD, InsMemRW;

    // ALU(ReadData1, ReadData2, ExtOut, sa, ALUSrcA, ALUSrcB, ALUOp, zero, result, sign)
    ALU alu(Data1, Data2, ExtOut, sa, ALUSrcA, ALUSrcB, ALUOp, zero, result, sign);
    // PC(PCWre, clk, Reset, pcin, ExtOut, pc)
    PC Pc(PCWre, clk, reset, pcnext, ExtOut, pc);
    // ControlUnit(Opcode, zero, sign, RegDst, InsMemRW, PCWre, ExtSel, DBDataSrc, mWR, mRD, ALUSrcA, ALUSrcB, PCSrc, ALUOp, RegWre)
    ControlUnit controlunit(Opcode, zero, sign, RegDst, InsMemRW, PCWre, ExtSel, DBDataSrc, mWR, mRD, ALUSrcA, ALUSrcB, PCSrc, ALUOp, RegWre);
    //  DataMEM(DAddr, DataIn, mRD, mWR, clk, DataOut)
    DataMEM dataMem(result, Data2, mRD, mWR, clk, DataOut);
    // InsMEM(IAddr, InsMemRW, op, rs, rt, rd, sa, imd, JumpAddr)
    InsMEM insMem(pc, InsMemRW, Opcode, rs, rt, rd, sa, immediate, JumpAddr);
    // RegisterFile(clk, rs, rt, rd, RegDst, result, DataOut, DBDataSrc, RegWre, ReadData1, ReadData2)
    RegisterFile registerfile(clk, rs, rt, rd, RegDst, result, DataOut, DBDataSrc, RegWre, Data1, Data2);
    // Extend(immediate, ExtSel, ExtOut)
    Extend extend(immediate, ExtSel, ExtOut);
    // Mux(pc, ExtOut, JumpAddress, PCSrc, nextpc)
    Mux mux(clk, pc, ExtOut, JumpAddr, PCSrc, PCWre, pcnext);
endmodule
```

## 仿真测试
&emsp;&emsp;以上我们实现了CPU的主要功能，接下来可以编写仿真测试文件对上面实现的内容进行仿真测试，以验证CPU的正确性。
仿真测试文件是sim文件，这里我命名为`test.v`。
```verilog
module test;

    // Inputs
    reg clk;
    reg reset;

    // Outputs
    wire[5:0] Opcode;
    wire[31:0] Data1;
    wire[31:0] Data2;
    wire[31:0] pc;
    wire[31:0] pcnext;
    wire[31:0] result;
    wire[4:0] rs;
    wire[4:0] rt;
    wire[31:0] DataOut;
    // uut
    s_cpu uut (
        .clk(clk),
        .reset(reset),
        .Opcode(Opcode),
        .Data1(Data1),
        .Data2(Data2),
        .pc(pc),
        .pcnext(pcnext),
        .result(result),
        .rs(rs),
        .rt(rt),
        .DataOut(DataOut)
    );

    initial
        begin
            // Initialize Inputs
            clk = 0;
            reset = 0;
            // Wait 100 ns for global reset finish
            #50
            reset = 1;

        end

        always
            begin
            #20;
            clk = ~clk;
            end
endmodule
```
&emsp;&emsp;这里只给出简单的而一些结果，具体的测试结果在这里就不详细叙述了，大家可以自己跑一下来看。
![测试结果](/images/ins5.png)

## 小结
&emsp;&emsp;以上就是单周期CPU的实现过程以及一些简单的分析。烧板的部分将在下一篇博客介绍，希望大家多多支持，谢谢！
