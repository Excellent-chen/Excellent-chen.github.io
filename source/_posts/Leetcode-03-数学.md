---
title: 03 数学
date: 2020-09-27 18:39:26
categories:
- Leetcode
tags:
- 数学
---

### 简单

-----

#### 191. 位1的个数

> ※ 该题目有很多种解法，比较常见的一种便是利用移位。然而，对于二进制表示中`0`较多的情况，会进行很多不必要的移位操作。为此，可以利用<a href="https://leetcode-cn.com/problems/hamming-distance/solution/yi-ming-ju-chi-by-leetcode/">布赖恩·克尼根</a>方法。

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int res = 0;
        while (n != 0) {
            res++;
            n = n & (n - 1);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

> **注**：对于该题目，在`Python`中，可以直接利用内置函数`bin(x ^ y).count('1')`；在`Java`中，可以直接利用内置函数`Integer.bitCount(x ^ y)`进行解决。

#### 231. 2的幂

> ※ 判断某个整数是否为`2`的幂，只需判断该整数的二进制表示中是否只有一个`1`，即`n & (n - 1)`是否为`0`。

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n == 0) {
            return false;
        }
        long x = (long) n;
        return (x & (x - 1)) == 0;
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

> **注**：刚开始做的时候没有考虑到`n = Integer.MIN_VALUE`的情况，对于该情况，`n & (n - 1)`等于`0`但是并不满足`2`的幂。为此，可以将`n`转换为长整型。

#### 461. 汉明距离

> ※ 该题目与`191. 位1的个数`类似，均利用了布赖恩·克尼根方法，只是多了一步与操作。

```java
class Solution {
    public int hammingDistance(int x, int y) {
        int res = 0, n = x ^ y;
        while (n != 0) {
            res++;
            n = n & (n - 1);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

-----

### 中等

-----

#### 319. 灯泡开关

> ※ 对于第`i`个灯泡，只有当`i`为完全平方数时，经过`N`轮之后其才会变为开启状态；这是因为完全平方数的因子数有奇数个，而灯泡经过奇数次操作之后会变为相反的状态。

```java
class Solution {
    public int bulbSwitch(int n) {
        return (int)Math.sqrt(n);
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

#### 338. 比特位计数

> ※ 对于`1`来说，其二进制表示中`1`的数目等于`0`的二进制表示中`1`的数目加`1`；对于`2`和`3`来说，它们的二进制表示中`1`的数目等于`0`和`1`的二进制表示中`1`的数目加`1`；对于`4`、`5`、`6`和`7`来说，它们的二进制表示中`1`的数目等于`0`、`1`、`2`和`3`的二进制表示中`1`的数目加`1`。找到该规律后，便可以利用动态规划进行解决。

```java
class Solution {
    public int[] countBits(int num) {
        int[] res = new int[num + 1];
        int d = 1;
        for (int i = 1; i <= num; i++) {
            res[i] = res[i - d] + 1;
            if ((i + 1) / 2 == d) {
                d *= 2;
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

-----