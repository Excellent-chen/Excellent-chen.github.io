---
title: 03 数学
date: 2020-09-27 18:39:26
categories:
- Leetcode
tags:
- 数学
---

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

-----