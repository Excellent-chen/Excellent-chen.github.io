---
title: 06 动态规划
date: 2020-10-24 16:26:43
categories:
- Leetcode
tags:
- 动态规划
---

### 中等

-----

#### 309. 最佳买卖股票时机含冷冻期

> ※ 令`f[i]`表示第`i`天结束后的累计最大收益，则`f[i]`对应了三种状态：目前持有一只股票，对应的累计最大收益为`f[i][0]`；目前不持有股票，并且处于冷冻期中，对应的累计最大收益为`f[i][1]`；目前不持有股票，并且未处于冷冻期中，对应的累计最大收益为`f[i][2]`。它们的状态转移方程分别如下：
>
> `f[i][0] = Math.max(f[i - 1][0], f[i - 1][2] - price[i])`；`f[i][1] = f[i - 1][0] + price[i]`；`f[i][2] = Math.max(f[i - 1][1], f[i - 1][2])`。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        if (n == 0) {
            return 0;
        }
        int f_0 = - prices[0], f_1 = 0, f_2 = 0;
        for (int i = 1; i < n; i++) {
            int new_f_0 = Math.max(f_0, f_2 - prices[i]);
            int new_f_1 = f_0 + prices[i];
            int new_f_2 = Math.max(f_1, f_2);
            f_0 = new_f_0;
            f_1 = new_f_1;
            f_2 = new_f_2;
        }
        return Math.max(f_1, f_2);
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 416. 分割等和子集

> ※ 该题目与常规`0-1`背包问题的区别在于：要求精确等于整个数组元素的和的一半，这使得对状态矩阵进行初始化时，所有的值都应该初始化为`false`。此外，对于一些特殊情况，可以直接判断出最终的结果，这一点需要注意。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int len = nums.length;
        // 1. 若 n < 2 则不能将数组分割成元素和相等的两个子集，直接返回 false
        if (len < 2) {
            return false;
        }
        int sum = 0, max = Integer.MIN_VALUE;
        for (int num : nums) {
            sum += num;
            max = Math.max(max, num);
        }
        // 2. 若 sum 为奇数或者 max > sum / 2，直接返回 false
        if (sum % 2 == 1 || max > sum / 2) {
            return false;
        }
        boolean[] dp = new boolean[sum / 2 + 1];
        dp[0] = true; // 若不选取任何正整数，则被选取的正整数等于零，这一步貌似挺重要的
        for (int i = 0; i < len; i++) {
            for (int j = sum / 2; j >= nums[i]; j--) {
                dp[j] |= dp[j - nums[i]]; // 注：每个数字只能使用一次，因此从后往前遍历是可以的！
            }
        }
        return dp[sum / 2];
    }
}
```

> ※ 时间复杂度：$O(N * target)$；空间复杂度：$O(target)$。

#### 486. 预测赢家

> ※ 定义行数与列数均等于数组长度的二维数组`dp`，其中，`dp[i][j]`表示当数组剩下的部分为下标`i`到下标`j`时，当前玩家与另一个玩家的分数之差的最大值。只有当`i <= j`时，数组剩下的部分才有意义，因此当`i > j`时，均有`dp[i][j] = 0`；当`i = j`时，只剩一个数字，当前玩家只能拿取该数字，因此对于所有`0 <= i < nums.length`，均有`dp[i][j] = nums[i]`；当`i < j`时，当前玩家可以选择`nums[i]`或`nums[j]`，然后轮到另一个玩家在数组剩下的部分选取数字。在上述两种方案中，当前玩家会选择最优的方案，使得自己的分数最大化。因此可以得到如下状态转移方程：`dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1])`，最后判断`dp[0][nums.length - 1]`的值，如果其大于或等于`0`，说明先手得分大于或等于后手得分，因此先手成为赢家，否则后手成为赢家。

```java
class Solution {
    public boolean PredictTheWinner(int[] nums) {
        int n = nums.length;
        int[][] dp = new int[n][n];
        for (int i = n - 1; i > -1; i--) {
            dp[i][i] = nums[i];
            for (int j = i + 1; j < n; j++) {
                dp[i][j] = Math.max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
            }
        }
        // System.out.println(Arrays.deepToString(dp));
        return dp[0][n - 1] >= 0;
    }
}
```

> ※ 时间复杂度：$O(N^2)$；空间复杂度：$O(N^2)$。

#### 516. 最长回文子序列

> ※ 建立二维数组`dp[s.length()][dp.length()]`，其中，`dp[i][j]`表示字符串第`i`个字符与第`j`个字符之间的所有字符构成的子字符串中最长回文子序列的长度（包括第`i`个字符和第`j`个字符）。易知，若`s.charAt(i) == s.charAt(j)`，则有`dp[i][j] = dp[i + 1][j - 1] + 2`；若`s.charAt(i) != s.charAt(j)`，则有`dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1])`。基于上述递推公式，可以得知，从后往前更新`dp`更为合适。

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int len = s.length();
        int[][] dp = new int[len][len];
        for (int i = len - 1; i > -1; i--) {
            dp[i][i] = 1;
            for (int j = i + 1; j < len; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][len - 1];
    }
}
```

> ※ 时间复杂度：$O(N^2)$；空间复杂度：$O(N^2)$。

#### 714. 买卖股票的最佳时机含手续费

> ※ 用变量`not_hold[i]`表示第`i`天不持有股票时的利润，`hold[i]`表示第`i`天持有股票时的理论，可得如下状态转移方程：
>
> `not_hold[i] = Math.max(not_hold[i - 1], hold[i - 1] + prices[i] - fee)`，
>
> `hold[i] = Math.max(hold[i], not_hold[i - 1] + prices[i])`，
>
> 在初始时，有`not_hold[0] = 0`，`hold[0] = - prices[0]`。

```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int not_hold = 0, hold = - prices[0];
        for (int i = 1; i < prices.length; i++) {
            int old_hold = not_hold;
            not_hold = Math.max(not_hold, hold + prices[i] - fee);
            hold = Math.max(hold, old_hold - prices[i]);
        }
        return not_hold;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 764. 最大加号标志

> ※ 预先计算每个中心点坐标在四个方向上对应的最长臂长$L_u$、$L_l$、$L_d$和$L_r$，则以该中心点坐标为中心可以构成的加号的阶为$min(L_u,L_l,L_d,L_r)$。特别的，对于每个中心点坐标`grid[i][j]`，若`grid[i][j] = 0`，则臂长为`0`；若`grid[i][j] = 1`，则臂长为当前方向上连续`1`的个数。

```java
class Solution {
    public int orderOfLargestPlusSign(int N, int[][] mines) {
        Set<Integer> banned = new HashSet<Integer>();
        for (int[] mine : mines) {
            banned.add(mine[0] * N + mine[1]);
        }
        int count, res = 0;
        int[][] dp = new int[N][N];
        for (int i = 0; i < N; i++) {
            // 计算每个位置左边有几个连续的一（包括自己）
            count = 0;
            for (int j = 0; j < N; j++) {
                count = banned.contains(i * N + j) ? 0 : count + 1;
                dp[i][j] = count;
            }
            // 计算每个位置右边有几个连续的一（包括自己）
            count = 0;
            for (int j = N - 1; j > -1; j--) {
                count = banned.contains(i * N + j) ? 0 : count + 1;
                dp[i][j] = Math.min(dp[i][j], count);
            }
        }
        for (int i = 0; i < N; i++) {
            count = 0;
            for (int j = 0; j < N; j++) {
                count = banned.contains(i + j * N) ? 0 : count + 1;
                dp[j][i] = Math.min(dp[j][i], count);
            }
            count = 0;
            for (int j = N - 1; j > -1; j--) {
                count = banned.contains(i + j * N) ? 0 : count + 1;
                dp[j][i] = Math.min(dp[j][i], count);
                res = Math.max(res, dp[j][i]);
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N^2)$；空间复杂度：$O(N^2)$。

#### 799. 香槟塔

> ※ 记录流入每个杯子的香槟的杯数之和`X`，若`X`大于`1`，那么将会有`(X - 1.0) / 2`的香槟流入下一层相邻的两个杯子中，依次类推，利用动态规划进行求解即可。

```java
class Solution {
    public double champagneTower(int poured, int query_row, int query_glass) {
        double[][] dp = new double[101][101];
        dp[0][0] = poured;
        for (int i = 0; i <= query_row; i++) {
            for (int j = 0; j <= i; j++) {
                if (dp[i][j] > 1.0) {
                    dp[i + 1][j] += (dp[i][j] - 1.0) / 2;
                    dp[i + 1][j + 1] += (dp[i][j] - 1.0) / 2;
                    dp[i][j] = 1.0;
                }
            }
        }
        return dp[query_row][query_glass];
    }
}
```

> ※ 时间复杂度：$O(N^2)$；空间复杂度：$O(N^2)$。

#### 983. 最低票价

> ※ 记第`N`天的最低票价为`F(N)`，假设已经求出了前`N - 1`天的最低票价，现在来求`F(N)`。如果第`N`天要选择有效期长度为`dayOfPass`的通行证，其价格为`cost`，那么在第`N - dayOfPass`天时就应该买入该通行证，才能使利益最大化；而我们已经求出了前`N - 1`天的最低票价，即`F(N - dayOfPass)`是已知的。因此，对于该选择，其花费为`F(N - dayOfPass) + cost`，取所有选择的最小值更新`F(N)`即可。

```java
class Solution {
    public int mincostTickets(int[] days, int[] costs) {
        int pos = 0, n = days.length;
        int[] dp = new int[days[n - 1] + 1];
        for (int i = 1; i < dp.length; i++) {
            if (days[pos] == i) {
                int d1 = i - 1 > 0 ? i - 1 : 0;
                int d2 = i - 7 > 0 ? i - 7 : 0;
                int d3 = i - 30 > 0 ? i - 30 : 0;
                dp[i] = Math.min(dp[d1] + costs[0], Math.min(dp[d2] + costs[1], dp[d3] + costs[2]));
                pos++;
            } else {
                dp[i] = dp[i - 1];
            }
        }
        return dp[dp.length - 1];
    }
}
```

> ※ 时间复杂度：$O(W)$，其中，`W`表示旅行计划中日期的最大值；空间复杂度：$O(W)$。

#### 1024. 视频拼接

> ※ 该题目与`55. 跳跃游戏`类似，只是多了一个转换的过程。

```java
class Solution {
    public int videoStitching(int[][] clips, int T) {
        int[] max = new int[T + 1]; // 保存以 i 为起点所能到达的最远距离。
        for (int[] clip : clips) {
            if(clip[0] < T) { // ???
                max[clip[0]] = Math.max(max[clip[0]], clip[1]);
            }
        }
        int res = 0, max_n = 0, next_max_n = 0;
        for (int i = 0; i <= T && i<= next_max_n; i++) {
            if (i > max_n) {
                res++;
                max_n = next_max_n;
            }
            next_max_n = Math.max(next_max_n, max[i]);
        }
        if (next_max_n < T) {
            return -1;
        } else {
        return res;
        }
    }
}
```

> ※ 时间复杂度：$O(T)$；空间复杂度：$O(T + N)$，其中，`T`表示区间的长度，`N`表示子区间的数量。

#### 1043. 分隔数组以得到最大和

> ※ 令`dp[i]`表示数组前`i`个元素（包含第`i`个元素）对应的结果，则`dp[i + 1] = max(dp[i - j] + max(arr[i + 1], ..., arr[i + 1 - j]) * (j + 1))`，其中`j = 1, ..., k`。

```java
class Solution {
    public int maxSumAfterPartitioning(int[] arr, int k) {
        int n = arr.length;
        int[] dp = new int[n + 1];
        for (int i = 0; i < n; i++) {
            int max = arr[i];
            for (int j = 0; j < k; j++) {
                if (i - j < 0) {
                    break;
                }
                max = Math.max(max, arr[i - j]);
                dp[i + 1] = Math.max(dp[i + 1], dp[i - j] + max * (j + 1));
            }
        }
        return dp[n];
    }
}
```

> ※ 时间复杂度：$O(NK)$；空间复杂度：$O(N)$。

#### 1049. 最后一块石头的重量 II

> ※ 该题目可以近似视为`0-1`背包问题，即将所有的石头分为两堆，并使得它们的总重相差最少（详情请见<a href="https://leetcode-cn.com/problems/last-stone-weight-ii/solution/you-qian-ru-shen-si-lu-ji-0-1-bei-bao-xiang-jie-mo/">解析</a>）。

```java
class Solution {
    public int lastStoneWeightII(int[] stones) {
        int sum = 0;
        for (int stone : stones) {
            sum += stone;
        }
        int[] dp = new int[sum / 2 + 1];
        for (int stone : stones) {
            for (int i = dp.length - 1; i >= stone; i--) {
                dp[i] = Math.max(dp[i], dp[i - stone] + stone); // 对空间进行压缩时需要倒序遍历！
            }
        }
        return sum - 2 * dp[sum / 2]; // 这里是 sum - 2 * dp[sum / 2] 而不是 sum - dp[sum / 2]
    }
}
```

> ※ 时间复杂度：$O(M * N)$，其中，`M`表示石块总重，`N`表示石块的个数；空间复杂度：$O(M)$。

#### 1143. 最长公共子序列

> ※ 令`dp[i][j]`表示字符串`S1[0:i]`与字符串`S2[0:j]`的最长公共子序列，则有`S1[i] = S2[j]`时，`dp[i][j] = dp[i - 1][j - 1]`；`S1[i] != S2[j]`时，`dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j])`。

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int rows = text1.length(), cols = text2.length();
        int[][] dp = new int[rows + 1][cols + 1];
        char[] chars1 = text1.toCharArray();
        char[] chars2 = text2.toCharArray();
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (chars1[i] == chars2[j]) {
                    dp[i + 1][j + 1] = dp[i][j] + 1;
                } else {
                    dp[i + 1][j + 1] = Math.max(dp[i][j + 1], dp[i + 1][j]);
                }
            }
        }
        return dp[rows][cols];
    }
}
```

> ※ 时间复杂度：$O(MN)$；空间复杂度：$O(MN)$。

#### 1314. 矩阵区域和

> ※ 用二维数组`dp`表示`mat`的前缀和，其中，`dp[i][j]`表示`mat`中以`(0, 0)`为左上角，`(i - 1, j - 1)`为右下角的子矩形的元素之和。题目需要对`mat`中的每个位置，计算以`(i - K, j - K)`为左上角，`(i + K, j + K)`为右下角的子矩形的元素之和，我们可以在前缀和的帮助下，通过：
>
> $sum = dp[i + K + 1][j + K + 1] - dp[i - K][j + K + 1] - dp[i + K + 1][j - K] + dp[i - K][j - K]$
>
> 得到元素之和。

```java
class Solution {
    public int[][] matrixBlockSum(int[][] mat, int K) {
        int rows = mat.length, cols = mat[0].length;
        int[][] dp = new int[rows + 1][cols + 1];
        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                dp[i][j] = mat[i - 1][j - 1] + dp[i - 1][j] + dp[i][j - 1] - dp[i - 1][j - 1];
            }
        }
        int[][] res = new int[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                int t = Math.max(0, i - K);
                int b = Math.min(rows - 1, i + K);
                int l = Math.max(0, j - K);
                int r = Math.min(cols - 1, j + K);
                res[i][j] = dp[b + 1][r + 1] + dp[t][l] - dp[b + 1][l] - dp[t][r + 1];
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(MN)$；空间复杂度：$O(MN)$。

-----

### 困难

-----

#### 174. 地下城游戏

> ※ 令`dp[i][j]`表示从坐标`(i, j)`到终点所需的最小初始值，即当我们到达坐标`(i, j)`时，若此时的路径和不小于`dp[i][j]`，便可到达终点。对于`dp[i][j]`，我们只需关心`dp[i][j + 1]`和`dp[i + 1][j]`的最小值。

```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        int m = dungeon.length, n = dungeon[0].length;
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i <= m; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }
        dp[m][n - 1] = dp[m - 1][n] = 1;
        for (int i = m - 1; i > -1; i--) {
            for (int j = n - 1; j > -1; j--) {
                int min = Math.min(dp[i][j + 1], dp[i + 1][j]);
                dp[i][j] = Math.max(1, min - dungeon[i][j]);
            }
        }
        return dp[0][0];
    }
}
```

> ※ 时间复杂度：$O(MN)$；空间复杂度：$O(MN)$。

-----