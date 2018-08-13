---
title: 多周期CPU设计与实现（最新版）
tags:
  - Verilog
abbrlink: 46129
date: 2018-08-10 12:25:30
---
## 概述
&emsp;&emsp;计组课程的第二个大实验--实现多周期CPU。相比起单周期，多周期的特点是：**一条指令的执行需要多个时钟周期**。也就是说，不是所有指令都需要完全执行五个阶段，绝大多数指令只需要执行其中的3-4个阶段就可以了，多周期CPU就是基于这种思想实现的。要看代码的直接看https://github.com/leungyukshing/-CPU-
<!-- more -->

## 分析
&emsp;&emsp;大家之前都完成了单周期CPU，对于CPU的工作原理和Verilog的模块化编程都有一定的了解，这里就不再赘述。我就直接分析一下多周期CPU的工作特点。一条指令需要多个时钟周期，就要用到**状态机**，用于控制一条指令中不同的状态，这个状态机是存在于控制单元里面的。
&emsp;&emsp;然后是一些数据的寄存器，用于保存数据。多周期CPU可能会导致**数据冲突**，我们必须提供一个部件存储数据的一个状态。

## 代码实现
### 1.控制单元（ControlUnti.v）
&emsp;&emsp;控制单元包含了状态机、设置下一个状态、设置信号三个重要部分。我们需要根据当前指令设置下一个状态，根据当前状态设置控制信号。这与单周期CPU是类似的，同样是需要先构建一个控制信号真值表。
![控制信号真值表](/images/控制信号真值表.png)

```verilog
module ControlUnit(
    input clk,
    input rst,
    input [5:0] Opcode,
    input zero,
    input sign,
    output reg PCWre,
    output reg ALUSrcA,
    output reg ALUSrcB,
    output reg DBDataSrc,
    output reg RegWre,
    output reg WrRegDSrc,
    output reg InsMemRW,
    output reg mRD,
    output reg mWR,
    output reg IRWre,
    output reg ExtSel,
    output reg [1:0] PCSrc,
    output reg [1:0] RegDst,
    output reg [2:0] ALUOp
    );

    reg[2:0] presentState;
    reg[2:0] nextState;
    reg [5:0] test;
    parameter [2:0] sIF = 3'b000,
                          sID = 3'b001,
                          sEXE = 3'b010,
                          sWB = 3'b011,
                          sMEM = 3'b100;
    initial begin
        nextState <= sID;
        test <= 2'b000000;
        ALUOp <= 2'b111;
    end

    // switch to next state
    always @(posedge clk) begin
        if (!rst)
            presentState <= sIF;
        else begin
            presentState <= nextState;
        end
    end

    parameter [5:0] add = 6'b000000,
                      sub = 6'b000001,
                      addi = 6'b000010,
                      Or = 6'b010000,
                      And = 6'b010001,
                      ori = 6'b010010,
                      sll = 6'b011000,
                      slt = 6'b100110,
                      sltiu = 6'b100111,
                      sw = 6'b110000,
                      lw = 6'b110001,
                      beq = 6'b110100,
                      bltz = 6'b110110,
                      j = 6'b111000,
                      jr = 6'b111001,
                      jal = 6'b111010,
                      halt = 6'b111111;

    // set next state
    always @(presentState or Opcode) begin
        case (presentState)
            sIF: nextState <= sID;
            sID: begin
                case (Opcode)
                    j,jr,halt: begin
                        nextState <= sIF;
                    end
                    jal: nextState <= sWB;
                    default: nextState <= sEXE;
                endcase
            end
            sEXE: begin
                case(Opcode)
                    beq,bltz: begin
                        nextState <= sIF;
                    end
                    sw,lw: begin
                        nextState <= sMEM;
                    end
                    default: nextState <= sWB;
                endcase
            end
            sMEM: begin
                case (Opcode)
                    sw: nextState <= sIF;
                    lw: nextState <= sWB;
                endcase
            end
            sWB: begin
                nextState <= sIF;
            end
        endcase
    end

    // set all signals
    always @(presentState or zero or Opcode or sign) begin
    // PCWre(Ê±ÖÓ´¥·¢)
        if ((presentState == 3'b011) || (Opcode == sw && presentState == 3'b100) || ((Opcode == beq || Opcode == bltz) && presentState == 3'b010) || (Opcode == jr && presentState == 3'b001) || (Opcode == j && presentState == 3'b001))
            PCWre = 1;
        else
            PCWre = 0;
    // ALUSrcA
        if (presentState == 3'b010 && Opcode == sll)
            ALUSrcA = 1;
        else
            ALUSrcA = 0;
    // ALUSrcB
        if (presentState == 3'b010 && (Opcode == addi || Opcode == ori || Opcode == sltiu || Opcode == lw || Opcode == sw))
            ALUSrcB = 1;
        else
            ALUSrcB = 0;
    // DBDataSrc
        if (presentState == 3'b100 && Opcode == lw)
            DBDataSrc = 1;
        else
            DBDataSrc = 0;
    // RegWre
        if (presentState == 3'b011 || (presentState == 3'b011 && Opcode == jal))
            RegWre = 1;
        else
            RegWre = 0;
    // WrRegDSrc
        if (presentState == 3'b011 && Opcode != jal)
            WrRegDSrc = 1;
        else
            WrRegDSrc = 0;
    // InsMemRW
        if (presentState == 3'b000)
            InsMemRW = 1;
        else
            InsMemRW = 0;
    // mRD
        if (presentState == 3'b100 && Opcode == lw)
            mRD = 1;
        else
            mRD = 0;
    // mWR
        if (presentState == 3'b100 && Opcode == sw)
            mWR = 1;
        else
            mWR = 0;
    // IRWre
        if (presentState == 3'b000)
            IRWre = 1;
        else
            IRWre = 0;
    // ExtSel
        if (presentState == 3'b010 && (Opcode == addi || Opcode == lw || Opcode == sw || Opcode == beq || Opcode == bltz))
            ExtSel = 1;
        else
            ExtSel = 0;
    // PCSrc[1:0]
        if (Opcode == j || Opcode == jal)
            PCSrc = 2'b11;
        else if (Opcode == jr)
            PCSrc = 2'b10;
        else if ((zero == 1 && Opcode == beq) || ((zero == 0 || sign == 1) && Opcode == bltz))
            PCSrc = 2'b01;
        else
            PCSrc = 2'b00;
    // RegDst[1:0]
        if (Opcode == jal)
            RegDst = 2'b00;
        else if (presentState == 3'b011 && (Opcode == addi || Opcode == ori || Opcode == sltiu || Opcode == lw))
            RegDst = 2'b01;
        else if (presentState == 3'b011 && (Opcode == add || Opcode == sub || Opcode == Or || Opcode == And || Opcode == slt || Opcode == sll))
            RegDst = 2'b10;
        else
            RegDst = 2'b11;
    // ALUOp[2:0]
    /*
        if (Opcode == add || Opcode == addi) begin
            ALUOp = 2'b000;
        end
        else if (Opcode == Or || Opcode == ori) begin
            ALUOp = 2'b101;
        end
        else
            ALUOp = 2'b111;
        */
        case(Opcode)
            add,addi,lw,sw:begin
                ALUOp = 3'b000;
            end
            sub,beq,bltz: begin
                test = 6'b010101;
                ALUOp = 3'b001;
            end
            sltiu: ALUOp = 3'b010;
            slt: ALUOp = 3'b011;
            sll: ALUOp = 3'b100;
            Or,ori:begin
                test <= 6'b111111;
                ALUOp <= 3'b101;
            end
            And: ALUOp = 3'b110;
            default: ALUOp = 3'b000;
        endcase

   end
endmodule
```

### 2.程序计数器（PC.v）
&emsp;&emsp;PC的设置是使用时钟触发的，我是用的是**上升沿触发**。

```verilog
module PC(
    input clk,
    input rst,
    input PCWre,
    input [1:0] PCSrc,
    input wire [31:0] addr,
    input wire [31:0] imd,
    input wire [31:0] ReadData1,
    output reg [31:0] PC,
    output reg [31:0] nextPC
    );

    initial begin
        PC = 0;
        nextPC = 4;
    end

    always @(posedge clk or negedge rst) begin
            if (!rst)
                PC = 0;
            else if (PCWre) begin
                PC = nextPC;
            end
        end

    always @(PCSrc or imd or addr) begin
         case(PCSrc)
                   2'b00: nextPC = PC + 4;
                   2'b01: nextPC = PC + 4 + imd * 4;
                   2'b10: nextPC = ReadData1;
                   2'b11: nextPC = addr;
          endcase
    end
endmodule
```

### 3.PC跳转地址合成（PCAdd.v）
&emsp;&emsp;对于跳转命令，指令中会有跳转的地址（一般是26位），我们需要合成一个32位的地址给PC。

```verilog
module PCAdd(
    input [25:0] addr,
    input [31:0] pc,
    output reg [31:0] nextpc
    );
    always@(addr) begin
        nextpc[31:28] = pc[31:28];
        nextpc[27:2] <= addr[25:0];
        nextpc[1] <= 0;
        nextpc[0] <= 0;
    end
endmodule
```

### 4.指令寄存器（InsMEM.v）
&emsp;&emsp;这里我使用了一种与单周期CPU不一样的方式初始化寄存器。我们可以首先编写好指令代码在一个txt文件中，在初始化的时候就将内容读入寄存器中。
测试指令：
![测试指令](/images/测试指令.png)
ins.txt文件内容：
![ins](/images/ins.png)

```verilog
module InsMEM(
    input clk,
    input InsMemRW,
    input IRWre,
    input [31:0] IAddr,
    output reg [31:0] DataOut
    );
    reg [31:0] temp; // ÖÐ¼ä±äÁ¿ ´æ´¢µØÖ·Êä³ö
    // Ö¸Áî¼Ä´æÆ÷Ä£¿é
    reg [7:0] ins[0:127];
    initial begin
        $readmemb("C:/Xilinx/Vivado/CPU2/CPU2.srcs/sources_1/new/ins.txt", ins);
    end

    always @(IAddr or InsMemRW) begin
        // Read Instruction
        if (InsMemRW) begin
            temp[31:24] = ins[IAddr];
            temp[23:16] = ins[IAddr+1];
            temp[15:8] = ins[IAddr+2];
            temp[7:0] = ins[IAddr+3];
        end
    end

    // IR
    always @(posedge clk) begin
        if(IRWre)
            DataOut <= temp;
    end
endmodule
```

### 5.数据存储器（DataMEM.v）
&emsp;&emsp;由于控制读写的信号与状态有关，因此数据存储器的读写就不需要使用时钟触发了。
```verilog
module DataMEM(
    input mRD,
    input mWR,
    input [31:0]DAddr,
    input [31:0]DataIn,
    output reg[31:0] DataOut
    );
    // ½¨Á¢Êý¾Ý´æ´¢Æ÷
    reg [7:0] memory[0:63];
    integer i;
    // Initialize the DataMEM
    initial begin
        for(i = 0; i <64; i=i+1)
            memory[i] <= 0;
    end
    initial begin
        DataOut <= 0;
    end

    always @(mRD or mWR) begin
        // Read
        if (mRD) begin
              DataOut[31:24] = memory[DAddr];
              DataOut[23:16] = memory[DAddr+1];
              DataOut[15:8] = memory[DAddr+2];
              DataOut[7:0] = memory[DAddr+3];
        end

        // Write
        if (mWR) begin
                memory[DAddr] <= DataIn[31:24];
                memory[DAddr+1] <= DataIn[23:16];
                memory[DAddr+2] <= DataIn[15:8];
                memory[DAddr+3] <= DataIn[7:0];
                DataOut <= 0;
        end
    end
endmodule
```

### 6.寄存器组（RegisterFile.v）
&emsp;&emsp;寄存器组的读写与单周期CPU类似，特别需要注意的是对于目标寄存器需要有一个数据选择器。
```verilog
module RegisterFile(
    input clk,
    input RegWre,
    input [1:0] RegDst,
    input [4:0] rs,
    input [4:0] rt,
    input [4:0] rd,
    input [31:0] WriteData,
    output [31:0] ReadData1,
    output [31:0] ReadData2
    );
    // ¼Ä´æÆ÷×é
    reg [31:0] registers[31:0];
    integer i;
    reg [4:0] WriteReg; // ÐèÒªÐ´Èë¼Ä´æÆ÷µÄµØÖ·

    // Initialize the registers
    initial begin
        for(i = 0; i < 32; i=i+1)
            registers[i] <= 0;
    end

    // Ð´¼Ä´æÆ÷Ñ¡Ôñ
    always @(RegDst or rt or rd) begin
        case(RegDst)
        2'b00: WriteReg = 5'b11111;
        2'b01: WriteReg = rt;
        2'b10: WriteReg = rd;
        default: WriteReg = 0;
        endcase
    end

    // Read
    assign ReadData1 = registers[rs];
    assign ReadData2 = registers[rt];

    // Write
    always @(negedge clk) begin
    // assign WriteData = (WrRegDSrc ? MemData : PC4);
        if ((WriteReg != 0) && (RegWre == 1))
            registers[WriteReg] <= WriteData;
    end

endmodule
```

### 7.算术逻辑运算单元（ALU.v）
&emsp;&emsp;这一部分和单周期CPU一样，不再赘述。
```verilog
module ALU(
    input ALUSrcA,
    input ALUSrcB,
    input [2:0] ALUOp,
    input [31:0] ReadData1,
    input [31:0] ReadData2,
    input [4:0] sa,
    input [31:0] ExtOut,
    output reg [31:0] result,
    output wire zero,
    output wire sign
    );

    // Á½¸ö²Ù×÷Êý±äÁ¿
    wire [31:0] A;
    wire [31:0] B;
    assign zero = (result ? 0 : 1);
    assign sign = result[31];
    // È·ÈÏµÚÒ»¸ö²Ù×÷Êý
    assign A = (ALUSrcA ? sa : ReadData1);
    // È·ÈÏµÚ¶þ¸ö²Ù×÷Êý
    assign B = (ALUSrcB ? ExtOut : ReadData2);

    always @(A or B or ALUOp) begin
        case(ALUOp)
            3'b000: result <= A + B;
            3'b001: result<=  A - B;
            3'b010: result <= (A < B) ? 1 : 0;
            3'b011: result <= (((A < B) && (A[31] == B[31])) || ((A[31] == 1 && B[31] == 0))) ? 1 : 0;
            3'b100: result <= B << A;
            3'b101: result <= A | B;
            3'b110: result <= A & B;
            3'b111: result <= A ^ B;
            default: result <= 0;
        endcase
    end
endmodule
```

### 8.符号位拓展模块（Extend.v）
&emsp;&emsp;这一部分与单周期CPU完全一致。
```verilog
module Extend(
    input ExtSel,
    input [15:0] imd,
    output reg [31:0] ExtOut
    );

    always @(imd or ExtSel) begin
        if (ExtSel) begin
            ExtOut <= {{16{imd[15]}}, imd[15:0]};
        end
        else begin
            ExtOut <= {{16{0}}, imd[15:0]};
        end
    end
endmodule
```

### 9.二选一选择器（Selector_2_to_1.v）
&emsp;&emsp;整个CPU中有多个地方用到了数据选择器，因此我把它单独分离出来。

```verilog
module Selector_2_to_1(
    input signal,
    input [31:0] A,
    input [31:0] B,
    output wire [31:0] out
    );
    assign out = (signal ? B : A);
endmodule

```

### 10.数据寄存器（DataRegister.v）
&emsp;&emsp;数据寄存器就是将数据保存一个时钟脉冲，用于解决数据冲突问题。
```verilog
module DataRegister(
    input clk,
    input [31:0] in,
    output reg [31:0] out
    );
    // Ê±ÖÓ´¥·¢
    always @(posedge clk) begin
            out = in;
    end
endmodule
```

### 11.顶层模块（m_cpu.v）
&emsp;&emsp;连接上述各个模块，成为一个多周期CPU。
```verilog
module m_cpu(
    input clk,
    input rst,
    output [5:0] Opcode,
    output [31:0] ADR_out,
    output [31:0] BDR_out,
    output [31:0] pc,
    output [31:0] pcnext,
    output [31:0] instruction,
    output [31:0] ALUoutDR_out,
    output [4:0] rs,
    output [4:0] rt,
    output [31:0] DBDR_out
    );

    wire [2:0] ALUOp;
    wire [31:0] DBDR_in;
    wire [31:0] ReadData1, ReadData2, result, DataOut;
    //wire [31:0] ADR_out, BDR_out, ALUoutDR_out, DBDR_out;
    wire [31:0] ExtOut;
    wire [31:0] JumpAddr;
    wire [15:0] immediate;
    wire [4:0] rd, sa;
    wire [1:0] PCSrc;
    wire [1:0] RegDst;
    wire zero, sign, PCWre, ALUSrcA, ALUSrcB, DBDataSrc, RegWre, WrRegDSrc, InsMemRW, mRD, mWR, IRWre, ExtSel;
    wire [31:0] WriteData;

    assign Opcode = instruction[31:26];
    assign rs = instruction[25:21];
    assign rt = instruction[20:16];
    // modules
    // PC(clk, rst, PCWre, PCSrc, addr, imd, ReadData1, pc)
    PC Pc(clk, rst, PCWre, PCSrc, JumpAddr, ExtOut, ReadData1, pc, pcnext);

    // ALU(ALUSrcA, ALUSrcB, ALUOp, ReadData1, ReadData2, sa, ExtOut, result, zero, sign)
    ALU alu(ALUSrcA, ALUSrcB, ALUOp, ADR_out, BDR_out, instruction[10:6], ExtOut, result, zero, sign);
    DataRegister ALUoutDR(clk, result, ALUoutDR_out);

    // DataMEM(mRD, mWR, DAddr, DataIn, DataOut)
    DataMEM dataMem(mRD, mWR, ALUoutDR_out, BDR_out, DataOut);
    Selector_2_to_1 selector2(DBDataSrc, result, DataOut, DBDR_in);
    DataRegister DBDR(clk, DBDR_in, DBDR_out);

    // RegisterFile
    Selector_2_to_1 selector3(WrRegDSrc, (pc+4), DBDR_out, WriteData);
    RegisterFile registerfile(clk, RegWre, RegDst, instruction[25:21], instruction[20:16], instruction[15:11], WriteData, ReadData1, ReadData2);
    DataRegister ADR(clk, ReadData1, ADR_out);
    DataRegister BDR(clk, ReadData2, BDR_out);

    // Extend(ExtSel, imd, ExtOut)
    Extend extend(ExtSel, instruction[15:0], ExtOut);

    // PCAdd(addr, pc, nextpc)
    PCAdd pcAdd(instruction[25:0], pc, JumpAddr);

    // InsMEM(clk, InsMemRW, IRWre, IAddr, DataOut)
    InsMEM insMem(clk, InsMemRW, IRWre, pc, instruction);

    // ControlUnit(clk, rst, Opcode, zero, sign, PCWre, ALUSrcA, ALUSrcB, DBDataSrc, RegWre, WrRegDSrc, InsMemRW, mRD, mWR, IRWre, ExtSel, PCSrc, RegDst, ALUOp)
    ControlUnit controlunit(clk, rst, instruction[31:26], zero, sign, PCWre, ALUSrcA, ALUSrcB, DBDataSrc, RegWre, WrRegDSrc, InsMemRW, mRD, mWR, IRWre, ExtSel, PCSrc, RegDst, ALUOp);


endmodule
```

## 仿真测试与烧板
### 仿真测试
&emsp;&emsp;以上就是多周期CPU的主要模块，编写测试文件对其进行仿真测试。
测试文件（test.v）：
```verilog
module test;

    // Inputs
    reg clk;
    reg rst;

    // Outputs
    wire [5:0] Opcode;
    wire [31:0] ReadData1, ReadData2, pc, pcnext, instruction, result, DataOut;
    wire [4:0] rs, rt;
    m_cpu uut (
        .clk(clk),
        .rst(rst),
        .Opcode(Opcode),
        .ADR_out(ReadData1),
        .BDR_out(ReadData2),
        .pc(pc),
        .pcnext(pcnext),
        .instruction(instruction),
        .ALUoutDR_out(result),
        .rs(rs),
        .rt(rt),
        .DBDR_out(DataOut)
        );

    initial
                begin
                    // Initialize Inputs
                    clk = 0;
                    rst = 0;
                    // Wait 100 ns for global reset finish
                    //clk = ~clk;
                    #50;
                    rst = 1;
                end

                always
                    begin
                    #20;
                    clk = ~clk;
                    end
endmodule
```
部分测试结果：
![仿真结果](/images/5.png)

### 烧板
&emsp;&emsp;烧板的过程和步骤与单周期CPU基本一致，因此这里就不再赘述，详情可以参考GitHub里面的代码或者是参考[单周期烧板介绍](https://leungyukshing.github.io/archives/%E5%8D%95%E5%91%A8%E6%9C%9FCPU%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0%EF%BC%88%E6%9C%80%E6%96%B0%E7%89%88%EF%BC%89--%E7%83%A7%E6%9D%BF%E9%83%A8%E5%88%86.html)。

## 小结
&emsp;&emsp;多周期CPU的难点在于对**指令状态转移的理解与实现**，其他部分与单周期CPU类似，过程中还是会遇到很多问题，需要大家给多点耐心去看波形调试，烧板的部分下一篇post再分享，谢谢大家支持！
