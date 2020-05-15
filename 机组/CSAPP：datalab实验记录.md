# CSAPP：datalab实验记录

## bitXor

```c
/* 
 * bitXor - x^y using only ~ and & 
 *   Example: bitXor(4, 5) = 1
 *   Legal ops: ~ &
 *   Max ops: 14
 *   Rating: 1
 */
```

这道题的意思就是限定符号实现异或。我们很容易就知道：
$$
a \oplus b = \overline a b + a \overline b
$$
再化简以下（逻辑代数的知识）：
$$
\overline a b + a \overline b = \overline { \overline {(a \overline b)}  \overline {(\overline a b)}}
$$
对照着实现就是：

```c
int bitXor(int x, int y) {
  return ~(~(x & ~y) & ~(~x & y));
}
```

## tmin

```c
/* 
 * tmin - return minimum two's complement integer 
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 4
 *   Rating: 1
 */
```

补码的最小值就是最高位位1咯，一个移位算法解决。

```c

int tmin(void) {
  return 1 << 31;
}
```

## isTMax

```c
/*
 * isTmax - returns 1 if x is the maximum, two's complement number,
 *     and 0 otherwise 
 *   Legal ops: ! ~ & ^ | +
 *   Max ops: 10
 *   Rating: 1
 */
```

这道题有点难度。

首先，`TMax` 取反一定是`TMin` 。`TMin` 的特点就是取反加一是相同的。

而有一个特殊值`0` 跟`TMin` 的特点是一样的，需要排除。

所以，首先判断是否等于，再将`0` 排除即可。

```c
int isTmax(int x) {
	x = ~x;
  return !((~x + 1) ^ x) & !!(x ^ 0);
}
```

## allOddBits

```c
/* 
 * allOddBits - return 1 if all odd-numbered bits in word set to 1
 *   where bits are numbered from 0 (least significant) to 31 (most significant)
 *   Examples allOddBits(0xFFFFFFFD) = 0, allOddBits(0xAAAAAAAA) = 1
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 2
 */
```

这道题，我们可以先构造一个常数`0xAAAAAAAA` ，与`x` 取与之后，再与这个常数判断是否相等即可。

```c
int allOddBits(int x) {
	int temp = (0xAA << 24) + (0xAA << 16) + (0xAA << 8) + 0xAA;
  return !((temp & x) ^ temp);
}
```

## negate

```c
/* 
 * isAsciiDigit - return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0' to '9')
 *   Example: isAsciiDigit(0x35) = 1.
 *            isAsciiDigit(0x3a) = 0.
 *            isAsciiDigit(0x05) = 0.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 15
 *   Rating: 3
 */
```

补码的取反加一。（提醒一下这个取反加1，不是因为首位是符号位，是因为首位是负权值位！！！）

```c
int negate(int x) {
  return (~x + 1);
}
```

## isAsciiDigit

```c
/* 
 * isAsciiDigit - return 1 if 0x30 <= x <= 0x39 (ASCII codes for characters '0' to '9')
 *   Example: isAsciiDigit(0x35) = 1.
 *            isAsciiDigit(0x3a) = 0.
 *            isAsciiDigit(0x05) = 0.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 15
 *   Rating: 3
 */
```

判断范围，一个数分别与这两个常数相减，一正一负就是在范围内。

正常思路是这样，然而直接这么实现的话，会有一个`0x80000030` 数据溢出，它与`0x39` 相减的时候溢出了。

嗯，所以我们不判断首位是否不同了。与`0x39` 相减的地方我们改成`-x` 加上`0x39` 。然后判断符号是否相同。就能避免溢出问题。

```c
int isAsciiDigit(int x) {
  return !((x + ~0x30 + 1) >> 31) & !((~x + 0x39 + 1) >> 31);
}
```

##  conditional

```c
/* 
 * conditional - same as x ? y : z 
 *   Example: conditional(2,4,5) = 4
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 16
 *   Rating: 3
 */
```

直接对x取非，就能得到对应的结果。再利用逻辑左移和算术右移，可以构造出一个`0x0` 或者是`0xffffffff` 。

就很简单啦~

```c
int conditional(int x, int y, int z) {
	x = (!x << 31) >>31;
  return (z & x) | (y & ~x);
}
```

## isLessOrEqual

```c
/* 
 * isLessOrEqual - if x <= y  then return 1, else return 0 
 *   Example: isLessOrEqual(4,5) = 1.
 *   Legal ops: ! ~ & ^ | + << >>
 *   Max ops: 24
 *   Rating: 3
 */
```

首先，先通过用符号判断大小，省点事。

符号相同时，再两者相减。判断一下符号位的正负即可。

```c
int isLessOrEqual(int x, int y) {
	int x_sign = x >> 31;
	int y_sign = y >> 31;
  return (x_sign & !y_sign) | (!(x_sign ^ y_sign) & !((y + ~x + 1) >> 31));
}
```

## logicalNeg

```c
/* 
 * logicalNeg - implement the ! operator, using all of 
 *              the legal operators except !
 *   Examples: logicalNeg(3) = 0, logicalNeg(0) = 1
 *   Legal ops: ~ & ^ | + << >>
 *   Max ops: 12
 *   Rating: 4 
 */
```

只有0是要返回1的。所以~

只要首位是1，就是负，返回1。

其余的所有正数加上`TMax` 都会溢出成负数。唯独0不会。

所以~加上`TMax` 再康康符号位即可~

```c
int logicalNeg(int x) {
	int Tmax = ~(0x1 << 31), sign = (x >> 31) & 0x1;
  return (sign | (((x + Tmax) >> 31) & 1)) ^ 0x1;
}
```

## howManyBits

```c
/* howManyBits - return the minimum number of bits required to represent x in
 *             two's complement
 *  Examples: howManyBits(12) = 5
 *            howManyBits(298) = 10
 *            howManyBits(-5) = 4
 *            howManyBits(0)  = 1
 *            howManyBits(-1) = 1
 *            howManyBits(0x80000000) = 32
 *  Legal ops: ! ~ & ^ | + << >>
 *  Max ops: 90
 *  Rating: 4
 */
```

这道题我不会，是参考别人的写法完成的。二分法非常巧妙啊~

最难理解的当然就是负数的情况。不过想想还是能想明白。

这里是利用二分法找到除了最高位以外的最高位。

整数很好说，关键是负数。由于补码的特性，前面是可以有很多没用的1的。所以负数是要找最高位的0哦~所以需要按照符号来取反。

```c
int howManyBits(int x) {
	int sign, b1, b2, b4, b8, b16;
	sign = (x >> 31);
	x = (sign & ~x) | (~sign & x);
	b16 = !!(x >> 16) << 4;
	x = x >> b16;
	b8 = !!(x >> 8) << 3;
	x = x >> b8;
	b4 = !!(x >> 4) << 2;
	x = x >> b4;
	b2 = !!(x >> 2) << 1;
	x = x >> b2;
	b1 = !!(x >> 1);
	x = x >> b1;
  return b16 + b8 + b4 + b2 + b1 + x + 1;
}
```

## floatScale2

```c
/* 
 * floatScale2 - Return bit-level equivalent of expression 2*f for
 *   floating point argument f.
 *   Both the argument and result are passed as unsigned int's, but
 *   they are to be interpreted as the bit-level representation of
 *   single-precision floating point values.
 *   When argument is NaN, return argument
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
 *   Max ops: 30
 *   Rating: 4
 */
```

分情况判断哦~

1. 如果是0，直接返回；
2. 如果阶码是0，直接左移一位。
3. 如果阶码是最大值，直接返回（NaN，不用处理~）。

接下来就是普通情况。阶码自加1。

再次判断，阶码达到最大值，直接返回不处理。

否则就将阶码拼接到原数据返回即可。

```c
unsigned floatScale2(unsigned uf) {
	int e = (uf & 0x7f800000) >> 23;
	int sign = uf & (0x1 << 31);
	int M = (uf << 9) >> 9;
	if(M == 0 && e == 0) return uf;
	if(e == 0x0) return (uf << 1) | sign;
	if(e == 0xff) return uf;
	e++;
	if(e == 0xff && M > 0) return uf;
  return sign | (e << 23) | M;
}
```

## floatFloat2Int

```c
/* 
 * floatFloat2Int - Return bit-level equivalent of expression (int) f
 *   for floating point argument f.
 *   Argument is passed as unsigned int, but
 *   it is to be interpreted as the bit-level representation of a
 *   single-precision floating point value.
 *   Anything out of range (including NaN and infinity) should return
 *   0x80000000u.
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. also if, while
 *   Max ops: 30
 *   Rating: 4
 */
```

根据浮点数的标准哈，将对应的数据分段拿下来~

嗯，如果是0直接返回0哈。

阶码处理一下，减一个偏移量。

如果阶码大于31，直接返回`0x80000000` 。

小于0，直接返回0.

而后面那一段数字，如果不处理的话，相当于就是自带有一个值为23的阶码。

所以，如果阶码大于23，就左移阶码多出的一段。小于23，就右移阶码少的一段。

```c
int floatFloat2Int(unsigned uf) {
	int sign = uf >> 31;
	int exp = ((uf & 0x7f800000) >> 23) - 127;
	int M = (uf & 0x007fffff) | 0x00800000;
	if(!(uf & 0x7fffffff)) return 0;
	if(exp > 31) return 0x80000000;
	if(exp < 0) return 0;
	if(exp >23) M <<= (exp - 23);
	else M >>= (23 - exp);
	if(!((M >> 31) ^ sign)) return M;
	else if(M >> 31) return 0x80000000;
	else return ~M + 1;
}
```

## floatPower2

```c
/* 
 * floatPower2 - Return bit-level equivalent of the expression 2.0^x
 *   (2.0 raised to the power x) for any 32-bit integer x.
 *
 *   The unsigned value that is returned should have the identical bit
 *   representation as the single-precision floating-point number 2.0^x.
 *   If the result is too small to be represented as a denorm, return
 *   0. If too large, return +INF.
 * 
 *   Legal ops: Any integer/unsigned operations incl. ||, &&. Also if, while 
 *   Max ops: 30 
 *   Rating: 4
 */
```

2的次方嘛~传入的参数就可以作为阶码了。

阶码加上偏移。然后，阶码小于0，返回0。阶码大于255，返回无限大，否则直接返回阶码左移23位的值。就刚好是对应的答案。因为当阶码不是0的时候自带一个1嗷~

```c
unsigned floatPower2(int x) {
	int inf;
	inf = 0xff << 23;
	x += 127;
	if(x <= 0) return 0x0;
	else if(x >= 255) return inf;
	return x << 23;
}
```



# 总结

## 位运算符号相等运算

这是在第三题学到的：

```c
int equals(int x, int y){
    return !(x ^ y);
}
```

