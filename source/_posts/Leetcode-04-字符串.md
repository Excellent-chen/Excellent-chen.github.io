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

#### 205. 同构字符串

> ※ 该题目很容易能够想到通过利用`Map`建立两个字符之间的映射关系来进行解决。需要注意的是，这种映射关系应具有对称性，如`ab`、`cc`便不具有对称性。

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        Map<Character, Character> map = new HashMap<Character, Character>();
        Map<Character, Character> rmap = new HashMap<Character, Character>();
        for (int i = 0; i < s.length(); i++) {
            if (map.containsKey(s.charAt(i))) {
                if (map.get(s.charAt(i)) != t.charAt(i)) {
                    return false;
                }
            } else {
                map.put(s.charAt(i), t.charAt(i));
            }
            if (rmap.containsKey(t.charAt(i))) {
                if (rmap.get(t.charAt(i)) != s.charAt(i)) {
                    return false;
                }
                
            } else {
                rmap.put(t.charAt(i), s.charAt(i));
            }
        }
        return true;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

> **注**：该题目也可以利用“第三者”建立两个字符串之间的一一对应关系，后面有时间可以试一下。

#### 290. 单词规律

> ※ 该题目与`205. 同构字符串`极为类似，在此不进行赘述。

```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        String[] str = s.split(" ");
        if (pattern.length() != str.length) {
            return false;
        }
        Map<Character, String> map = new HashMap<Character, String>();
        Map<String, Character> rmap = new HashMap<String, Character>();
        for (int i = 0; i < pattern.length(); i++) {
            if (map.containsKey(pattern.charAt(i))) {
                if (!map.get(pattern.charAt(i)).equals(str[i])) {
                    return false;
                }
            } else {
                map.put(pattern.charAt(i), str[i]);
            }
            if (rmap.containsKey(str[i])) {
                if (rmap.get(str[i]) != pattern.charAt(i)) {
                    return false;
                }
            } else {
                rmap.put(str[i], pattern.charAt(i));
            }
        }
        return true;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

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