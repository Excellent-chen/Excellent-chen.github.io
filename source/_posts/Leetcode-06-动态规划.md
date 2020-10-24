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

-----