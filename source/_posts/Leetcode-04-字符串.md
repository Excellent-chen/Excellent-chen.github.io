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

-----