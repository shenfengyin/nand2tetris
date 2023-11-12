# CHANGELOG

## 实验一：从Nand构建门

> 基于Nand作为最基础的布尔逻辑，衍生出其他所有逻辑

### 基础门
> 与、非、或、选择器、异或、Demultiplexor
- And(a,b) = Nand(Nand(a,b), Nand(a,b))
- Not a = Nand(a, a)
- Or(a,b) = Nand(Nand(a,a),Nand(b,b)) = Nand(Not(a), Not(b))
- Mux = Or(And(b, sel), And(a, Not(sel)))
- Xor = Or(And(a, Not(b)), And(Not(a), b))
- out: a = And(in, Not(sel)), b = And(in, sel)

### 多位基本门（Multi-Bit）
- And16：for i = 0..15: out[i] = (a[i] and b[i])
- Not16：for i=0..15: out[i] = not in[i]
- Or16：for i = 0..15 out[i] = (a[i] or b[i])
- Mux16：for i = 0..15 out[i] = a[i] if sel == 0 ，b[i] if sel == 1

### 多通道逻辑门（multi-Way）
- Or8Way：out = (in[0] or in[1] or ... or in[7])
- Mux8Way16： out = a if sel == 00
                   b if sel == 01
                   c if sel == 10
                   d if sel == 11
- DMux8Way：同理 2 ^ selNum = outNum

## 实验二：构建加法器和ALU

> 基于先前构建的逻辑门，构建加法器和ALU

### 加法器
- 半加器（HalfAdder）：实现两数（单比特）加法，输出sum和carry
- 全加器（FullAdder）：实现三数（单比特）加法，输出sum和carry
  - 为了处理 add1 + add2 + carry（低位的进位）
- 16位加法器（Add16）：两数（16比特）加法，输出sum，不考虑溢出问题
- 增量器（Inc16）：16-bit数自增1

可以看出，基础逻辑门（Xor + And)可以实现半加器，两个半加器+异或逻辑可以实现全加器。
半加器+全加器又可以实现16位加法器。

### 算术逻辑单元（ALU）
算术逻辑单元（Arithmetic Logic Unit，简称ALU）是计算机中的一个重要组件，主要用于执行各种算术和逻辑操作。

它通常是中央处理器（CPU）的一部分，负责执行加法、减法、逻辑与、逻辑或、逻辑非等基本操作。
在计算机中，ALU是执行算术和逻辑操作的核心部件之一。

ALU通常包含多个输入端口和一个输出端口，它能够根据输入信号执行不同的操作，并将结果输出。

因为全部操作有6个控制位引起的，所以在一定的设计下可以生成2^6 = 64个函数逻辑，见下：


```
/**
 * The ALU (Arithmetic Logic Unit).
 * Computes one of the following functions:
 * x+y, x-y, y-x, 0, 1, -1, x, y, -x, -y, !x, !y,
 * x+1, y+1, x-1, y-1, x&y, x|y on two 16-bit inputs, 
 * according to 6 input bits denoted zx,nx,zy,ny,f,no.
 * In addition, the ALU computes two 1-bit outputs:
 * if the ALU output == 0, zr is set to 1; otherwise zr is set to 0;
 * if the ALU output < 0, ng is set to 1; otherwise ng is set to 0.
 */

// Implementation: the ALU logic manipulates the x and y inputs
// and operates on the resulting values, as follows:
// if (zx == 1) set x = 0        // 16-bit constant
// if (nx == 1) set x = !x       // bitwise not
// if (zy == 1) set y = 0        // 16-bit constant
// if (ny == 1) set y = !y       // bitwise not
// if (f == 1)  set out = x + y  // integer 2's complement addition
// if (f == 0)  set out = x & y  // bitwise and
// if (no == 1) set out = !out   // bitwise not
// if (out == 0) set zr = 1
// if (out < 0) set ng = 1

CHIP ALU {
    IN  
        x[16], y[16],  // 16-bit inputs        
        zx, // zero the x input?
        nx, // negate the x input?
        zy, // zero the y input?
        ny, // negate the y input?
        f,  // compute out = x + y (if 1) or x & y (if 0)
        no; // negate the out output?

    OUT 
        out[16], // 16-bit output
        zr, // 1 if (out == 0), 0 otherwise
        ng; // 1 if (out < 0),  0 otherwise

    PARTS:
    //具体实现看代码逻辑
```