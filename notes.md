---
layout: post
title: "csapp笔记"
date: 2021.02.19
categories: references
tag: [notes,tutorials,computer]
mathjax: true
---

#   Machine-Level Representation

## Bits, Bytes, Integer

### Unsigned

#### Can be very tricky

```c
#define DELTA sizeof(int) // returns size_t, unsigned
int i;
// i-DELTA casted to unsigned, comparison failed
for (i = CNT;i-DELTA >= 0; i-=DelTA)
	...
```

#### Proper way to use unsigned as loop index

```c
size_t i;
for (i = cnt -; i < cnt; i--) // fails when overflow
    a[i] += a[i+1];
```

> use size_t instead of unsigned is better

### Signed Integer

examples(***overflow***):

#### Addition

- `-1`+`-1`

  ```c
  0xFF+0xFF = 0x1FE = 0xFE = -2
  ```

- `-1`+`2`

  ```c
  0xFF+0x02 = 0x101 = 0x01 = 1
  ```

#### Multiplication

- `2`*`-1`

  ```c
  0x02*0xFF = 0x1FE = 0xFE = -2
  ```

- `-1`*`-2`

  ```c
  0xFF*0xFE = 0xFD02 = 0x02 = 2
  ```

#### Multiply by the power of 2

```c
3*4 = 0b11 * 0b100 = 0b1100 = 3<<2
```

**much faster** than traditional multiplication

#### Negation

```c
-x = ~x + 1
// example:
-1 = 0b1111 = ~0b0001 + 1 = ~1 + 1
1 = 0b0001 = ~0b1111 + 1 = ~-1 + 1
```

### Shift

#### Right Shift

#### Two Kinds

```c
0b1110 >> 1 = 0b0111 // logical shift
0b1110 >> 1 = 0b1111 // arithmetic shift
///NOTE: In Java, logical shift is >>>
```

> There's **NO** requirement for c  compiler to perform which kind of right shifts

###  Encoding

#### Big and Small Endian 

example: encoding 4-byte value of `0x01234567`

```c
01 23 45 67 // big endian
67 45 23 01 // small endian, much more popular
```

## Floating Point(IEEE)

### General Representation

$$
(-1)^sM2^E
$$

![](https://www.player7.cc:44343/uploads/big/0b8a004e3974ab550ddb22bc1ea896f8.png)

- $$E = Exp - Bias$$
- $$Bias = 2^{k-1}-1$$
- $$M = frac+1<<23$$

### Denormalized Number

- exp = 0, frac = 0
  - represents zero
  - note +0 and -0 since `s` exists
- exp = 0, frac != 0
  - numbers closest to 0.0
  -  E = 1-Bias, same to exp = 1
  - equispaced
- exp = 11...111, frac = 0
  - represents value infinity
  - operation that overflows
  - both positive and negative
  - E.g., 1.0/0.0=+inf, 1.0/-0.0 = -inf
- exp = 11...111, frac != 0
  - not a number(NaN)
  - represents case when no numeric value can be determined

![img](https://www.player7.cc:44343/uploads/big/ea49405ac64e07ff4ff2011664015aff.png)

### Examples

#### 平滑过渡

![image-20210220141323026](/mnt/share/Programming/notes/source/_posts/2021.02.19csapp.assets/image-20210220141323026.png)

#### Distribution

![image-20210220141824901](/mnt/share/Programming/notes/source/_posts/2021.02.19csapp.assets/image-20210220141824901.png)

![image-20210220141951238](/mnt/share/Programming/notes/source/_posts/2021.02.19csapp.assets/image-20210220141951238.png)

### Operations

#### Rounding

- Rounding Modes

  - towards zero

  - round down

  - round up

  - nearest even (default)

    ![image-20210220143259824](/mnt/share/Programming/notes/source/_posts/2021.02.19csapp.assets/image-20210220143259824.png)

    ![image-20210220143511204](/mnt/share/Programming/notes/source/_posts/2021.02.19csapp.assets/image-20210220143511204.png)

 