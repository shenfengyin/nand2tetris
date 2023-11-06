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