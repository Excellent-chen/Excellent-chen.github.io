---
title: 04 字符串
date: 2020-09-28 11:13:28
categories:
- Leetcode
tags:
- 字符串
---

### 简单

-----

#### 28. 实现 strStr()

> ※ 借助双指针，该题目便能够轻松解决。

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if (needle.length() == 0) {
            return 0;
        }
        for (int i = 0; i <= haystack.length() - needle.length(); i++) {
            while (i <= haystack.length() - needle.length() && haystack.charAt(i) != needle.charAt(0)) {
                i++;
            }
            if (i > haystack.length() - needle.length()) {
                return -1;
            }
            int j;
            for (j = i; j < i + needle.length(); j++) {
                if (haystack.charAt(j) != needle.charAt(j - i)) {
                    break;
                }
            }
            if (j == i + needle.length() && haystack.charAt(j - 1) == needle.charAt(needle.length() - 1)) {
                return i;
            }
        }
        return -1;
    }
}
```

> ※ 时间复杂度：最坏为$O((n-l)l)$；空间复杂度：$O(1)$。

#### 389. 找不同

> ※ 该题目与`136. 只出现一次的数字`类似，均利用了异或的以下性质：任何数和自身进行异或运算均为`0`；任何数和`0`进行异或运算均为自身；异或运算满足交换律。只是没有想到的是，字符居然也能进行异或运算。

```java
class Solution {
    public char findTheDifference(String s, String t) {
        // 字符串也是可以进行异或运算(^)的
        char res = t.charAt(t.length() - 1);
        for (int i = 0; i < s.length(); i++) {
            res ^= s.charAt(i);
            res ^= t.charAt(i);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(n)$；空间复杂度：$O(1)$。

-----