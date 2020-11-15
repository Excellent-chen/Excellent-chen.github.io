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