---
title: PL0语言编译程序
abbrlink: 62485
date: 2019-01-19 17:55:15
tags:
---

## Intro

&emsp;&emsp;PL0语言是Pascal语言的一个子集，这里我们给出PL0的编译程序和一个测试程序。目的是修改PL0编译程序使得它可以运行，然后运行测试程序，输出中间运行代码和结果。如果你还没有配置Pascal编译环境，请点击[传送门](https://leungyukshing.github.io/archives/Pascal编译环境搭建.html)

<!-- more -->

&emsp;&emsp;PL0小知识：

1. PL0语言是瑞士计算机科学家N.Wirth为教学目的设计的一门语言；
2. 它是一个比较早期的程序设计语言，与C语言历史差不多长；
3. 它的特点是接近自然语言（英语），直观、易于理解。

---

## 内容

1. 在本机上实现PL0语言的编译程序。
2. 扩展PL0语言的功能，支持用户的输入和输出。

完整代码请到GitHub查看：https://github.com/leungyukshing/Compiler-Project

---

## Part1

第一部分的任务只要是修改源程序的错误，使之可以运行即可。

### 文件流修改

第一行的`program PL0(input, output)`可以删掉，因为后面我们用到了文件输入。后面有一行`page(output)`也删掉，因为我们没有`page()`这个过程。

在一开始的var变量声明中加入三个变量`fin， fout: text;`，这两个变量代表文件的输入流和输出流。`fin`全局替换`input`，`fout`全局替换`output`。然后再加上`sfile, dfile: string`，这两个变量存放的是文件名。

然后在最下面的主程序中加入下面的代码，支持用户输入源程序文件名和输出结果的文件名。

```pascal
  writeln('please input source program file name : ');
  readln(sfile);
  assign(fin,sfile);
  reset(fin);
  writeln('please input the file name to save result : ');
  readln(dfile);
  assign(fout,dfile);
  rewrite(fout);
```

然后在整个程序的每一个`writeln()`后加上`writeln(fout, )`即可。`writeln()`就是在命令行输出，而`writeln(fout, )`就是输入到文件中。

### 符号的修改

源程序的部分符号是中文字符，只需要全部替换成英文符号即可。

### goto语句

直接删掉goto语句，没有影响。

### 变量名替换

部分变量名与Pascal的保留字冲突的，因此需要替换。比如`object = (constant, variable, procedure);`,object和procedure都是Pascal的保留字，因此需要替换。`object`换成`object1`，`procedure`换成`procedur`。这部分的代码不能全局替换，要自己判断哪些是真正的保留字，哪些是变量名。

### 运算符修改

运算符的问题主要出现在三个比较符号

```pascal
  ssym[‘≠’] := neq;
  ssym[‘≤’] := leq;      
  ssym[‘≥’] := geq;
```

我们需要用两个符号去表示这三种关系：

+ `≠`改成`<>`
+ `≤`改成`<=`
+ `≥`改成`>=`

然后把上面三条语句删掉，但是为了保持编译器能够处理这三个关系，我们需要在逻辑上对代码进行修改，要让编译器正确识别出这三种关系。在识别`:=`的语句后，模仿类似的代码编写：

```pascal
 else if ch = '<'
        then
        begin
          getch;
          if ch = '='
            then
            begin
              sym := leq;
              getch
            end
          else if ch = '>'
                then
                begin
                  sym := neq;
                  getch
                end
          else sym := lss
        end
  else if ch = '>'
        then
        begin
          getch;
          if ch = '='
            then
            begin
              sym := geq;
              getch
            end
          else sym := gtr;
      end
```

### 大小写修改

将关键字全部改成小写：

```pascal
  word[1] := 'begin     '; word[2] := 'call      ';
  word[3] := 'const     '; word[4] := 'do        ';
  word[5] := 'end       '; word[6] := 'if        ';
  word[7] := 'odd       '; word[8] := 'procedure ';
  word[9] := 'then      '; word[10] := 'var       ';
  word[11] := 'while     ';
```

### 添加Not

在下面这几个语句加`not`：

1.

```pascal
while not eoln(fin) do
```

2.

```pascal
until not (ch in ['a'..'z', '0'..'9']);
```

3.

```pascal
until not (ch in ['0'..'9']);
```

4.

```pascal
procedure  test(s1, s2 : symset; n : integer);
begin
  if not (sym in s1)
    then
    begin
      error(n);
      s1 := s1 + s2;
    while not (sym in s1) do
      getsym
  end
end {test};
```

5.

```pascal
if not (sym in [eql, neq, lss, leq, gtr, geq])
```

6.

```pascal
until Not (sym in declbegsys);
```



至此，第一部分的任务就已经完成了。

---

## Part2

第二部分的扩展功能主要是增加读入和输出的功能。即添加read和write语句。

### 修改保留字个数

修改保留字的个数，添加两个：

```pascal
const
  norw = 13; {保留字的个数}
  txmax = 100; {标识符表长度}
  nmax = 14; {数字的最大位数}
  al = 10; {标识符的长度}
  amax = 2047; {最大地址}
  levmax = 3; {程序体嵌套的最大深度}
  cxmax = 200; {代码数组的大小}
```

在保留字中添加两个保留字read和write：

```pascal
  word[1] := 'begin     '; word[2] := 'call      ';
  word[3] := 'const     '; word[4] := 'do        ';
  word[5] := 'end       '; word[6] := 'if        ';
  word[7] := 'odd       '; word[8] := 'procedure ';
  word[9] := 'read      '; word[10] := 'then      ';
  word[11] := 'var       ';
  word[12] := 'while     '; word[13] := 'write     ';
  
  wsym[1] := beginsym;   wsym[2] := callsym;
  wsym[3] := constsym;   wsym[4] := dosym;
  wsym[5] := endsym;    wsym[6] := ifsym;
  wsym[7] := oddsym;    wsym[8] := procsym;
  wsym[9] := readsym;
  wsym[10] := thensym;    wsym[11] := varsym;
  wsym[12] := whilesym;  wsym[13] := writesym;
  
  mnemonic[lit] := 'LIT  ';    mnemonic[opr] := 'OPR  ';
  mnemonic[lod] := 'LOD  ';    mnemonic[sto] := 'STO  ';
  mnemonic[cal] := 'CAL  ';    mnemonic[int] := 'INT  ';
  mnemonic[jmp] := 'JMP  ';    mnemonic[jpc] := 'JPC  ';
  mnemonic[red] := 'RED  ';    mnemonic[wrt] := 'WRT  ';
```

### 添加对应的解释程序

在读入到read和write时，要添加对这两个命令解释逻辑：

```pascal
red :
              begin
                {writeln('Input an integer: ');
                writeln(fout, 'Input an integer: ');}
                write('$ :');
                write(fout, '$ :');
                readln(s[base(l) + a]);
                writeln(fout, s[base(l) + a]);
                {writeln(fout, s[base(l) + a]);}
              end;
          wrt:
              begin
                write('Here is the result: ');
                writeln(s[t]);
                write(fout, 'Here is the result: ');
                writeln(fout, s[t]);
                t := t + 1;
              end
```

在condition里面添加：

```pascal
  else if sym = readsym
    then
    begin
      getsym;
      if sym = lparen
        then
        repeat
          getsym;
          if sym = ident
            then
            begin
              i := position(id);
              if i = 0
                then error(11)
              else if table[i].kind <> variable
                then
                begin
                  error(12);
                  i := 0
                end
              else with table[i] do
                gen(red, lev - level, adr)
            end
          else error(4);
          getsym;
        until sym <> comma
      else error(40);
      if sym <> rparen
        then error(22);
      getsym
    end
  else if sym = writesym
    then
    begin
      getsym;
      if sym = lparen
        then
        begin
          repeat
            getsym;
            expression([rparen, comma] + fsys);
            gen(wrt, 0, 0);
          until sym <> comma;
          if sym <> rparen
            then error(22);
          getsym
        end
      else error(40)
    end;
```

### 修改源程序

在源程序中添加输入输出语句。

```pascal
const  m = 7, n = 85;
var  x, y, z, q, r;

procedure  multiply;
var  a, b;
begin  a := x;  b := y;  z := 0;
  while b > 0 do
    begin  
      if odd b
        then z := z + a;
      a := 2*a ;  b := b/2 ;
    end
end;

procedure  divide;
var  w;
begin  r := x;  q := 0;  w := y;
  while w <= r do
    w := 2*w ;
  while w > y do
    begin  q := 2*q;  w := w/2;
      if w <= r
      then
        begin
          r := r - w;
          q := q + 1
        end
    end
end;

procedure  gcd;
var f, g ;
begin
  f := x;
  g := y;
  while f <> g do
    begin
      if f < g
        then g := g - f;
      if g < f
        then f := f - g;
    end;
  z := f
end;

begin
  read(x);
  read(y);
  call multiply;
  write(z);

  read(x);
  read(y);
  call divide;
  write(q);

  read(x);
  read(y);
  call gcd;
  write(z);
end.
```

## 运行结果

### Part1

中间代码输出：

```
   2  INT    0    5
   3  LOD    1    3
   4  STO    0    3
   5  LOD    1    4
   6  STO    0    4
   7  LIT    0    0
   8  STO    1    5
   9  LOD    0    4
  10  LIT    0    0
  11  OPR    0   12
  12  JPC    0   29
  13  LOD    0    4
  14  OPR    0    6
  15  JPC    0   20
  16  LOD    1    5
  17  LOD    0    3
  18  OPR    0    2
  19  STO    1    5
  20  LIT    0    2
  21  LOD    0    3
  22  OPR    0    4
  23  STO    0    3
  24  LOD    0    4
  25  LIT    0    2
  26  OPR    0    5
  27  STO    0    4
  28  JMP    0    9
  29  OPR    0    0
  31  INT    0    4
  32  LOD    1    3
  33  STO    1    7
  34  LIT    0    0
  35  STO    1    6
  36  LOD    1    4
  37  STO    0    3
  38  LOD    0    3
  39  LOD    1    7
  40  OPR    0   13
  41  JPC    0   47
  42  LIT    0    2
  43  LOD    0    3
  44  OPR    0    4
  45  STO    0    3
  46  JMP    0   38
  47  LOD    0    3
  48  LOD    1    4
  49  OPR    0   12
  50  JPC    0   72
  51  LIT    0    2
  52  LOD    1    6
  53  OPR    0    4
  54  STO    1    6
  55  LOD    0    3
  56  LIT    0    2
  57  OPR    0    5
  58  STO    0    3
  59  LOD    0    3
  60  LOD    1    7
  61  OPR    0   13
  62  JPC    0   71
  63  LOD    1    7
  64  LOD    0    3
  65  OPR    0    3
  66  STO    1    7
  67  LOD    1    6
  68  LIT    0    1
  69  OPR    0    2
  70  STO    1    6
  71  JMP    0   47
  72  OPR    0    0
  74  INT    0    5
  75  LOD    1    3
  76  STO    0    3
  77  LOD    1    4
  78  STO    0    4
  79  LOD    0    3
  80  LOD    0    4
  81  OPR    0    9
  82  JPC    0  100
  83  LOD    0    3
  84  LOD    0    4
  85  OPR    0   10
  86  JPC    0   91
  87  LOD    0    4
  88  LOD    0    3
  89  OPR    0    3
  90  STO    0    4
  91  LOD    0    4
  92  LOD    0    3
  93  OPR    0   10
  94  JPC    0   99
  95  LOD    0    3
  96  LOD    0    4
  97  OPR    0    3
  98  STO    0    3
  99  JMP    0   79
 100  LOD    0    3
 101  STO    1    5
 102  OPR    0    0
 103  INT    0    8
 104  LIT    0    7
 105  STO    0    3
 106  LIT    0   85
 107  STO    0    4
 108  CAL    0    2
 109  LIT    0   25
 110  STO    0    3
 111  LIT    0    3
 112  STO    0    4
 113  CAL    0   31
 114  LIT    0   84
 115  STO    0    3
 116  LIT    0   36
 117  STO    0    4
 118  CAL    0   74
 119  OPR    0    0
```



运行栈输出：

```
START PL/0
7
85
7
85
0
7
14
42
28
21
35
56
10
112
5
147
224
2
448
1
595
896
0
25
3
25
0
3
6
12
24
48
0
24
1
1
2
12
4
6
8
3
84
36
84
36
48
12
24
12
12
END PL/0
```

### Part2

中间代码：

```
   2  INT    0    5
   3  LOD    1    3
   4  STO    0    3
   5  LOD    1    4
   6  STO    0    4
   7  LIT    0    0
   8  STO    1    5
   9  LOD    0    4
  10  LIT    0    0
  11  OPR    0   12
  12  JPC    0   29
  13  LOD    0    4
  14  OPR    0    6
  15  JPC    0   20
  16  LOD    1    5
  17  LOD    0    3
  18  OPR    0    2
  19  STO    1    5
  20  LIT    0    2
  21  LOD    0    3
  22  OPR    0    4
  23  STO    0    3
  24  LOD    0    4
  25  LIT    0    2
  26  OPR    0    5
  27  STO    0    4
  28  JMP    0    9
  29  OPR    0    0
  31  INT    0    4
  32  LOD    1    3
  33  STO    1    7
  34  LIT    0    0
  35  STO    1    6
  36  LOD    1    4
  37  STO    0    3
  38  LOD    0    3
  39  LOD    1    7
  40  OPR    0   13
  41  JPC    0   47
  42  LIT    0    2
  43  LOD    0    3
  44  OPR    0    4
  45  STO    0    3
  46  JMP    0   38
  47  LOD    0    3
  48  LOD    1    4
  49  OPR    0   12
  50  JPC    0   72
  51  LIT    0    2
  52  LOD    1    6
  53  OPR    0    4
  54  STO    1    6
  55  LOD    0    3
  56  LIT    0    2
  57  OPR    0    5
  58  STO    0    3
  59  LOD    0    3
  60  LOD    1    7
  61  OPR    0   13
  62  JPC    0   71
  63  LOD    1    7
  64  LOD    0    3
  65  OPR    0    3
  66  STO    1    7
  67  LOD    1    6
  68  LIT    0    1
  69  OPR    0    2
  70  STO    1    6
  71  JMP    0   47
  72  OPR    0    0
  74  INT    0    5
  75  LOD    1    3
  76  STO    0    3
  77  LOD    1    4
  78  STO    0    4
  79  LOD    0    3
  80  LOD    0    4
  81  OPR    0    9
  82  JPC    0  100
  83  LOD    0    3
  84  LOD    0    4
  85  OPR    0   10
  86  JPC    0   91
  87  LOD    0    4
  88  LOD    0    3
  89  OPR    0    3
  90  STO    0    4
  91  LOD    0    4
  92  LOD    0    3
  93  OPR    0   10
  94  JPC    0   99
  95  LOD    0    3
  96  LOD    0    4
  97  OPR    0    3
  98  STO    0    3
  99  JMP    0   79
 100  LOD    0    3
 101  STO    1    5
 102  OPR    0    0
 103  INT    0    8
 104  RED    0    3
 105  RED    0    4
 106  CAL    0    2
 107  LOD    0    5
 108  WRT    0    0
 109  RED    0    3
 110  RED    0    4
 111  CAL    0   31
 112  LOD    0    6
 113  WRT    0    0
 114  RED    0    3
 115  RED    0    4
 116  CAL    0   74
 117  LOD    0    5
 118  WRT    0    0
 119  OPR    0    0
```

输入与输出：

```
START PL/0
$ :4
$ :5
Here is the result: 20
$ :80
$ :4
Here is the result: 20
$ :15
$ :27
Here is the result: 3
END PL/0
```

---

## Github项目

github项目地址：https://github.com/leungyukshing/Compiler-Project

---

## 总结

&emsp;&emsp;老师一开始给出的源程序代码比较多错误，花了不少时间来修改。希望这篇博客能够帮助到你，谢谢！