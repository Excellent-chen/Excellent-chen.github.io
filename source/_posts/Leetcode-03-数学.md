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

#### 326. 3的幂

> ※ 在整型范围内，`3`的整数幂最大为`1162261467`，即`3`的`19`次幂。由于`3`为质数，因此$3^{19}$的除数只有$3^{0}$、$3^{1}$、...、$3^{19}$，因此，只需判断$3^{19}$除以`n`余数是否为`0`即可。

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

> **注**：对于求解`n`的幂，应该怎么办呢？这里给出两种方法：
>
> <a href="https://leetcode-cn.com/problems/power-of-three/solution/3de-mi-by-leetcode/">基准转换法</a>，首先将`num`转换为`n`进制，然后利用正则表达式判断转换后的数字是否以`1(^1)`开头，后面跟着`0`或多个`0(0*)`。该种方法的时间复杂度为$O(log_{n}num)$，空间复杂度为$O(log_{n}num)$。
>
> <a href="https://leetcode-cn.com/problems/power-of-three/solution/3de-mi-by-leetcode/">运算法</a>，若`num`为`n`的幂，则$i=log_{n}num=log_a(num)/log_a{n}$为整数，因此，只需判断`i%1`是否为整数即可。该种方法的时间复杂度为`Unknown`（取决于`Math.log()`的耗时），空间复杂度为$O(1)$。需要注意的是，`Math.log()`返回的是`double`类型的数值，可能会存在一定的误差，我们还需要将该误差考虑在内。

#### 342. 4的幂

> ※ 该题目与`231. 2的幂`类似，均利用了布赖恩·克尼根方法，只是多了一步与操作，用于判断数字为`2`的偶数次幂还是奇数次幂。

```java
class Solution {
    public boolean isPowerOfFour(int num) {
        return num > 0 && (num & (num - 1)) == 0 && ((num & 0xaaaaaaaa) == 0);
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

#### 441. 排列硬币

> ※ 该题目利用一元二次方程的求根公式即可轻松解决，不过需要注意的是`Math.sqrt()`函数的参数范围。

```java
class Solution {
    public int arrangeCoins(int n) {
        // 当 n = 1804289383 时 Math.sqrt(2 * n) 将会溢出，具体为什么俺也不清楚。。。
        return (int) (Math.sqrt(2) * Math.sqrt(n + 0.125) - 0.5);
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

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

#### 476. 数字的补数

> ※ 对于正整数`1`，其去掉前缀`0`之后的二进制表示形式为`1`，对应的补数为`0`；对于正整数`5`，其去掉前缀`0`之后的二进制表示形式为`101`，对应的补数为`010`；可以发现，`0 + 1 = 1`、`101 + 010 = 111`。因此，可以事先计算出输入正整数去掉前缀`0`之后的二进制表示形式的位数`count`，再令`count`个`1`减去正整数即可得到它的补数。

```java
class Solution {
    public int findComplement(int num) {
        int count = 0, base = 0;
        while (base < num) {
            count++;
            base = (base << 1) + 1;
        }
        return (1 << (count - 1)) - 1 + (1 << (count - 1)) - num;
    }
}
```

> ※ 时间复杂度：$O(logN)$；空间复杂度：$O(1)$。

> **注**：在解决该题目时，应时刻防止数字溢出！

#### 504. 七进制数

> ※ 该题目比较简单，不过需要注意的是当`n = 0`的特殊情况！

```java
class Solution {
    public String convertToBase7(int num) {
        if (num == 0) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        int sign = num >= 0 ? 1 : -1;
        num *= sign;
        while (num != 0) {
            sb.append(String.valueOf(num % 7));
            num /= 7;
        }
        if (sign == -1) {
            sb.append("-");
        }
        sb.reverse();
        return sb.toString();
    }
}
```

> ※ 时间复杂度：$O(logN)$；空间复杂度：$O(1)$吧。

#### 728. 自除数

> ※ 首先想到的方法便是暴力求解，但是感觉暴力求解太`LOW`了。本以为题解会有更加牛逼的方法，没想到用的居然也是暴力法。。。

```java
class Solution {
    public List<Integer> selfDividingNumbers(int left, int right) {
        List<Integer> res = new ArrayList<Integer>();
        for (int num = left; num <= right; num++) {
            int n = num;
            while (n != 0) {
                if (n % 10 == 0 || num % (n % 10) != 0) {
                    break;
                }
                n /= 10;
            }
            if (n == 0) {
                res.add(num);
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

-----

### 中等

-----

#### 89. 格雷编码

> ※ 设`n`阶格雷编码集合为`G(n)`，则`G(n+1)`阶格雷编码为：给`G(n)`阶格雷编码中的每个元素的二进制表示前添加前缀`0`；倒序为`G(n)`阶格雷编码中的每个元素的二进制表示前添加前缀`1`。根据上述规律，即可从`0`阶格雷编码推导至任意阶格雷编码。

```java
class Solution {
    public List<Integer> grayCode(int n) {
        List<Integer> res = new ArrayList<Integer>();
        res.add(0);
        int head = 1;
        for (int i = 0; i < n; i++) {
            for (int j = res.size() - 1; j > -1; j--) {
                res.add(head + res.get(j));
            }
            head <<= 1;
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N^2)$；空间复杂度：$O(1)$。

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