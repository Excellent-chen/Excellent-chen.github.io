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

#### 371. 两整数之和

> ※ 利用异或`^`运算（`0001^0010=0011`），可以得到两个整数相加后的无进位结果；利用与`&`运算（`(0001 & 0010) << 1 = 0000`），可以得到两个整数相加时的进位信息。利用上述性质即可在不使用`+`的前提下计算出两整数之和。

```java
class Solution {
    public int getSum(int a, int b) {
        while (b != 0) {
            int sum = a ^ b;
            int car = (a & b) << 1;
            a = sum;
            b = car;
        }
        return a;
    }
}
```

> ※ 时间复杂度：可能是$O(N)$吧；空间复杂度：$O(1)$。

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

#### 860. 柠檬水找零

> ※ 每当收到一张`5`元钞票时，将`5`元钞票的数量加一；收到一张`10`元钞票时，将`10`元钞票的数量加一，`5`元钞票的数量减一；收到一张`20`元钞票时，优先使用一张`5`元钞票和一张`10`元钞票找零，若没有足够的`10`元钞票，再考虑使用三张`5`元钞票。在上述过程中，若出现了剩余钞票数量不足的情况，则说明无法找零。

```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int nums_of_five = 0, nums_of_ten = 0;
        for (int i = 0; i < bills.length; i++) {
            if (bills[i] == 5) {
                nums_of_five++;
            } else if (bills[i] == 10) {
                if (nums_of_five == 0) {
                    return false;
                } else {
                    nums_of_five--;
                    nums_of_ten++;
                }

            } else {
                if (nums_of_five > 0 && nums_of_ten > 0) {
                    nums_of_five--;
                    nums_of_ten--;
                } else if (nums_of_five > 2) {
                    nums_of_five -= 3;
                } else {
                    return false;
                }
            }
        }
        return true;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 883. 三维形体投影面积

> ※ 对于三维形体的俯视图投影面积，等于“高度”不为`0`的元素的个数；对于三维形体的主视图投影面积，等于每一行元素的最大值之和；对于三维形体的侧视图投影面积，等于每一列元素的最大值之和。为了获取每一行和每一列元素的最大值，最先想到的是遍历二维数组两次的方式，后来在题解中发现了一种更为巧妙的方法。

```java
class Solution {
    public int projectionArea(int[][] grid) {
        int res = 0;
        // 每一行每一列的最大值相加，再加上不为 0 的元素的个数
        for (int i = 0; i < grid.length; i++) {
            int row_max = 0, col_max = 0;
            for (int j = 0; j < grid.length; j++) {
                if (grid[i][j] > 0) {
                    res++;
                }
                row_max = Math.max(row_max, grid[i][j]); // 妙
                col_max = Math.max(col_max, grid[j][i]); // 啊
            }
            res += (row_max + col_max);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(MN)$；空间复杂度：$O(1)$。

#### 1512. 好数对的数目

> ※ 遍历所有可能的`(i, j)`二元组，并判断它们对应的数值是否满足`nums[i] == nums[j]`即可。

```java
class Solution {
    public int numIdenticalPairs(int[] nums) {
        int res = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] == nums[j]) {
                    res++;
                }
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N^2)$；空间复杂度：$O(N)$。

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

#### 357. 计算各个位数不同的数字个数

> ※ 比较简单的排列组合类型问题，只是存在一些边界情况需要考虑，如`n = 0`或`n > 10`，前者有`10`种可能结果，后者始终有`8877691`种可能结果（因为只有`10`个阿拉伯数字，第`10+`位无论是哪个数字定会重复）。

```java
class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        // if (n == 0) {
        //     return 1;
        // } else {
        //     int res = 10;
        //     int a = 9, b = 9;
        //     int count = n > 10 ? 10 : n; // 很重要！！！
        //     for (int i = 1; i < count; i++) {
        //         res += (a * b);
        //         a *= b--;
        //         // b--;
        //     }
        //     return res;
        // }
        if (n == 0) {
            return 1;
        } else if (n >= 10) {
            return 8877691;
        } else {
            int res = 10;
            int a = 9, b = 9;
            for (int i = 1; i < n; i++) {
                res += (a * b);
                a *= b--;
            }
            return res;
        }
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

#### 396. 旋转函数

> ※ `F(k + 1)`与`F(k)`之间存在如下关系：
>
> ```shell
> F(0) = 0 * A[0] + 1 * A[1] + 2 * A[2] + ... + (n - 1) * A[n - 1]
> F(1) = 0 * A[n - 1] + 1 * A[0] + 2 * A[1] + ... + (n - 1) * A[n - 2]
> F(2) = 0 * A[n - 2] + 1 * A[n - 1] + 2 * A[0] + ... + (n - 1) * A[n - 3]
> F(n - 1) = 0 * A[1] + 1 * A[2] + 2 * A[3] + ... + (n - 1) * A[0]
> 
> F(1) - F(0) = A[0] + A[1] + ... + A[n - 1] - n * A[n - 1]
> F(2) - F(1) = A[0] + A[1] + ... + A[n - 1] - n * A[n - 2]
> F(n - 1) - F(n - 2) = A[0] + A[1] + ... + A[n - 1] - n * A[1]
> 
> F(1) = F(0) + SUM - n * A[n -1]
> F(2) = F(1) + SUM - n * A[n -2]
> F(n - 1) = F(n - 2) - n * A[1]
> ```

```java
class Solution {
    public int maxRotateFunction(int[] A) {
        int SUM = 0, F_0 = 0;
        for (int i = 0; i < A.length; i++) {
            SUM += A[i];
            F_0 += i * A[i];
        }
        int max = F_0, n = A.length;
        for (int i = 1; i < n; i++) {
            int F_1 = SUM + F_0 - n * A[n - i];
            max = Math.max(max, F_1);
            F_0 = F_1;
        }
        return max;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 454. 四数相加 II

> ※ 将四个数组分为两部分，`A`和`B`一组，`C`和`D`一组。对于`A`和`B`，使用二重循环对它们进行遍历，得到所有的`A[i] + B[j]`并放入哈希表中。对于哈希表中的每个键值对，键表示一种`A[i] + B[j]`，值表示对应键出现的次数。对于`C`和`D`，同样使用二重循环对它们进行遍历，当遍历到`C[i] + D[j]`时，若哈希表中出现了`-(C[i] + D[j])`，那么将`-(C[i] + D[j])`在哈希表中出现的次数进行累加即可。

```java
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int a : A) {
            for (int b : B) {
                map.put(a + b, map.getOrDefault(a + b, 0) + 1);
            }
        }
        int res = 0;
        for (int c : C) {
            for (int d : D) {
                if (map.containsKey(- c - d)) {
                    res += map.get(-c - d);
                }
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N^2)$；空间复杂度：$O(N^2)$，最坏情况下，`A[i] + B[j]`的值各不相同。

#### 672. 灯泡开关 Ⅱ

> ※ 该题目容易得知第`i`个灯泡的状态始终与第`i + 6`个灯泡的状态一致，但经过<a href="[为什么前三个灯泡就可以确定其他灯泡的状态 - 灯泡开关 Ⅱ - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/bulb-switcher-ii/solution/wei-shi-yao-qian-san-ge-deng-pao-jiu-ke-yi-que-din/)">证明</a>可以进一步得知所有灯泡的状态只与前三个灯泡的状态有关。对于前三个灯泡，至多对应`8`种不同的状态，枚举一下即可：当`m = 0`时，所有灯泡均亮起，在这种情况下，答案总为`1`；当`m = 1`时，分别执行`a, b, c, d`，可以得到`(0, 0, 0), (1, 0, 1), (0, 1, 0), (0, 1, 1)`四个状态，在这种情况下，`n = 1, 2, 3`对应的答案分别为`2, 3, 4`；当`m = 2`时，可以得到除`(0, 1, 1)`之外的`7`个状态，在这种情况下，`n = 1, 2, 3`对应的答案分别为`2, 4, 7`；当`m = 3`时，可以得到所有状态， 在这种情况下，`n = 1, 2, 3`对应的答案分别为`2, 4, 8`。

```java
class Solution {
    public int flipLights(int n, int m) {
        n = Math.min(n, 3);
        if (m == 0) return 1;
        if (m == 1) return n == 1 ? 2 : n == 2 ? 3 : 4;
        if (m == 2) return n == 1 ? 2 : n == 2 ? 4 : 7;
        return n == 1 ? 2 : n == 2 ? 4 : 8;
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

#### 738. 单调递增的数字

> ※ 首先从高位开始找到第一个非升序的位，将此位减`1`，后续所有位改为`9`；然后从此位开始往前判断看更改后是否满足升序要求，若不满足则把本位也提升到`9`，前一位继续减`1`，直到满足升序条件为止。

```java
class Solution {
    public int monotoneIncreasingDigits(int N) {
        int i = 1;
        char[] strN = String.valueOf(N).toCharArray();
        while (i < strN.length && strN[i - 1] <= strN[i]) {
            i++;
        }
        if (i < strN.length) {
            while (i > 0 && strN[i - 1] > strN[i]) {
                strN[i - 1] -= 1;
                i--;
            }
            for (i += 1; i < strN.length; i++) {
                strN[i] = '9';
            }
        }
        return Integer.parseInt(String.valueOf(strN));
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 807. 保持城市天际线

> ※ 该题目与`883. 三维形体投影面积`类似，均利用了投影的性质。首先，对二维数组进行遍历，求出每一行、每一列的最大值；然后，再次对二维数组进行遍历，对于每一个元素`grid[i][j]`，其可以增加的最大高度等于`Math.min(row_max[i], col_max[j]) - grid[i][j]`；最后，将所有元素可以增加的最大高度累加在一起，得到最终结果。

```java
class Solution {
    public int maxIncreaseKeepingSkyline(int[][] grid) {
        int row = grid.length, col = grid[0].length;
        int[] row_max = new int[row];
        int[] col_max = new int[col];
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                row_max[i] = Math.max(row_max[i], grid[i][j]);
                col_max[j] = Math.max(col_max[j], grid[i][j]);
            }
        }
        int res = 0;
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                res += Math.min(row_max[i], col_max[j]) - grid[i][j];
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N^2)$；空间复杂度：$O(N)$。

#### 1637. 两点之间不包含任何点的最宽垂直面积

> ※ 该题目比较简单，对`points`进行排序，比较两个相邻`point`的`x`坐标之间的差值，并保留最大差值即可。

```java
class Solution {
    public int maxWidthOfVerticalArea(int[][] points) {
        int res = 0;
        Arrays.sort(points, (P1, P2) -> P1[0] - P2[0]); // 如何对二维数组进行排序？
        for (int i = 0; i < points.length - 1; i++) {
            if (points[i + 1][0] - points[i][0] > res) {
                res = points[i + 1][0] - points[i][0];
            }
        }
        return res;
    }
}
```

```python
class Solution:
    def maxWidthOfVerticalArea(self, points: List[List[int]]) -> int:
        res = 0
        points.sort()
        for i in range(len(points) - 1):
            if points[i + 1][0] - points[i][0] > res:
                res = points[i + 1][0] - points[i][0]
        return res
```

> ※ 时间复杂度：取决于内部函数；空间复杂度：取决于内部函数。

#### 1642. 可以到达的最远建筑

> ※ 优先利用梯子去爬高度相差较大的楼层，当梯子不够时，再利用砖块去爬。基于上述思想，可以利用优先队列。

```java
class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        int i = 0,  n = heights.length;
        Queue<Integer> queue = new PriorityQueue<Integer>();
        while (i + 1 < n) {
            int diff = heights[i + 1] - heights[i];
            if (diff > 0) {
                queue.offer(diff);
            }
            if (queue.size() > ladders) {
                bricks -= queue.poll();
            }
            if (bricks < 0) {
                return i;
            }
            i++;
        }
        return n - 1;
    }
}
```

> ※ 时间复杂度：取决于内部函数；空间复杂度：取决于内部函数。

-----