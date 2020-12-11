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

> ※ 时间复杂度：最坏为$O((N-L)L)$；空间复杂度：$O(1)$。

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

#### 242. 有效的字母异位词

> ※ 换句话说，该题目是想让我们判断两个字符串中每个字符出现的次数是否相等，我们只需计算两个字符串中每个字符出现的次数并进行比较即可。

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] counter = new int[26];
        for (int i = 0; i < s.length(); i++) {
            counter[s.charAt(i) - 'a']++;
            counter[t.charAt(i) - 'a']--;
        }
        for (int i = 0; i < 26; i++) {
            if (counter[i] != 0) {
                return false;
            }
        }
        return true;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

> **注**：若数组的字符串包含`Unicode`字符，只需将`counter`设置为`Map`即可。

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

#### 387. 字符串中的第一个唯一字符

> ※ 首先，遍历字符串，将字符串中的每个字符及其出现的次数保存至`Map`中；然后，再次遍历字符串，并判断当前遍历字符是否只出现一次，若是，则返回当前下标；若始终没有找到只出现一次的字符，则返回`-1`。

```java
class Solution {
    public int firstUniqChar(String s) {
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        for (int i = 0; i < s.length(); i++) {
            map.put(s.charAt(i), map.getOrDefault(s.charAt(i), 0) + 1);
        }
        for (int i = 0; i < s.length(); i++) {
            if (map.get(s.charAt(i)) == 1) {
                return i;
            }
        }
        return -1;
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

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 434. 字符串中的单词数

> ※ 在解决该题目时，上来直接调用了`s.split(" ").length`，得到了错误结果。阅读<a href="https://leetcode-cn.com/problems/number-of-segments-in-a-string/solution/zi-fu-chuan-zhong-de-dan-ci-shu-by-leetcode/">题解</a>后得知，该题目存在一些边缘情况，如：开头或结尾存在一个或多个空格，这需要我们事先调用`trim()`函数将它们剔除；在单词之间可能存在一个以上的空格，调用`split()`函数时将会得到多个空字符`""`，这需要我们利用正则表达式将它们剔除。

```java
class Solution {
    public int countSegments(String s) {
        s = s.trim();
        if (s.length() == 0) {
            return 0;
        } else {
            // 原来 split() 函数中也可以使用正则表达式。
            // 注：这里是 \\s+ 而不是 //s+
            return s.split("\\s+").length;
        }
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

> **注**：如果是用`Python`做，这道题只需要一行代码：`return len(s.split())`。

#### 709. 转换成小写字母

> ※ 没什么可说的，遍历字符串中的每个字符，若当前字符为大写字母，将其转换为小写字母即可。

```java
class Solution {
    public String toLowerCase(String str) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            if ('A' <= c && c <= 'Z') {
                sb.append(String.valueOf((char) (c + 32)));
            } else {
                sb.append(String.valueOf(c));
            }
        }
        return sb.toString();
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 771. 宝石与石头

> ※ 首先，遍历字符串`J`，并利用哈希集合存储其中的每个字符；然后，遍历字符串`S`，判断其中的每个字符是否出现在哈希集合中，若出现，则是宝石。

```java
class Solution {
    public int numJewelsInStones(String J, String S) {
        Set<Character> set = new HashSet<Character>();
        for (int i = 0; i < J.length(); i++) {
            set.add(J.charAt(i));
        }
        int res = 0;
        for (int i = 0; i < S.length(); i++) {
            if (set.contains(S.charAt(i))) {
                res++;
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(M+N)$；空间复杂度：$O(N)$。

#### 821. 字符的最短距离

> ※ 首先，从左向右遍历字符串，记录上一个字符`C`出现的位置`pre`，并记录`i - pre`；然后，从右向左遍历字符串，记录下一个字符`C`出现的位置`post`，并记录`post - i`；则字符的最短距离即为`Math.min(i - pre, post - i)`。

```java
class Solution {
    public int[] shortestToChar(String S, char C) {
        int[] res = new int[S.length()];
        int pre = -10000;
        for (int i = 0; i < S.length(); i++) {
            if (S.charAt(i) == C) {
                pre = i;
            }
            res[i] = i - pre;
        }
        int post = 10000;
        for (int i = S.length() - 1; i > -1; i--) {
            if (S.charAt(i) == C) {
                post = i;
            }
            res[i] = Math.min(res[i], post - i);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 844. 比较含退格的字符串

> ※ 首先能想到的方法便是利用栈进行解决，在查看<a href="https://leetcode-cn.com/problems/backspace-string-compare/solution/bi-jiao-han-tui-ge-de-zi-fu-chuan-by-leetcode-solu/">题解</a>之后，发现可以借助双指针将空间复杂度优化至`O(1)`。

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int p = 0, q = 0;
        int i = S.length() - 1, j = T.length() - 1;
        while (i >= 0 || j >= 0) {
            while (i >= 0) {
                if (S.charAt(i) == '#') {
                    p++;
                } else if (p > 0) {
                    p--;
                } else {
                    break;
                }
                i--;
            }
            while (j >= 0) {
                if (T.charAt(j) == '#') {
                    q++;
                } else if (q > 0) {
                    q--;
                } else {
                    break;
                }
                j--;
            }
            if (i >= 0 && j >= 0) {
                if (S.charAt(i) != T.charAt(j)) {
                    return false;
                }
            } else {
                if (i >= 0 || j >= 0) {
                    return false;
                }
            }
            i--;
            j--;
        }
        return true;
    }
}
```

> ※ 时间复杂度：$O(M + N)$；空间复杂度：$O(1)$。

#### 925. 长按键入

> ※ 由<a href="https://leetcode-cn.com/problems/long-pressed-name/solution/chang-an-jian-ru-by-leetcode-solution/">题解</a>知，字符串`typed`中的每个字符，有且只有两种用途：（1）作为`name`字符串的一部分，此时会匹配`name`中的一个字符；（2）作为长按键入的一部分，此时应与前一个字符相同。若`typed`中存在一个字符使得以上两个条件均不满足，则应当直接返回`false`；否则，当`typed`扫描完毕后，再检查`name`中的每个字符是否都被匹配了即可。

```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {
        int i = 0, j = 0;
        // 这里只需要判断 j 是否小于 typed.length() 即可，无需再判断 i 是否小于 name.length()。
        while (j < typed.length()) {
            if (i< name.length() && name.charAt(i) == typed.charAt(j)) {
                i++;
                j++;
            } else if (j > 0 && typed.charAt(j - 1) == typed.charAt(j)) {
                j++;
            } else {
                return false;
            }
        }
        // 这里不应该直接返回 true，而是应该判断 i 是否已经遍历完了 name 中所有的字符。
        // 若直接返回 true，则测试用例 name = pyplrz，typed = ppyypllr 将会返回 true。
        return i == name.length();
    }
}
```

> ※ 时间复杂度：$O(M + N)$；空间复杂度：$O(1)$。

-----

### 中等

-----

#### 151. 翻转字符串里的单词

> ※ 首先，利用`trim()`函数和`split("\\s+")`函数剔除首尾冗余的空格和单词之间冗余的空格；然后，借助`StringBuilder`对字符串数组进行反转拼接，没拼接完一个单词后在后面加上分隔用的空格；最后，将结尾处冗余的空格剔除。

```java
class Solution {
    public String reverseWords(String s) {
        String[] str = s.trim().split("\\s+");
        StringBuilder res = new StringBuilder(str.length);
        for (int i = str.length - 1; i > -1; i--) {
            res.append(str[i]);
            res.append(" ");
        }
        return res.toString().trim();
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 402. 移掉K位数字

> ※ 对于两个相同长度的数字序列，最左边不同的数字决定了这两个数字的大小，例如，对于`A = 1axxx`，`B = 1bxxx`，若`a > b`，则`A > B`。基于此，可以得知，若要使剩下的数字最小，需要保证靠前的数字尽可能小。给定一个长度为`n`的数字序列$[D_0D_1D_2...D_{n-1}]$，从左往右找到第一个位置`i (i > 0)`，使得$D_i < D_{i-1}$，并删去$D_{i-1}$；如果不存在，说明整个数字序列单调不减，删去最后一个数字即可。

```java
class Solution {
    public String removeKdigits(String num, int k) {
        int count = 0;
        Deque<Character> queue = new LinkedList<Character>();
        for (char c : num.toCharArray()) {
            // 对于每个数字，如果该数字小于栈顶元素，我们就不断弹出栈顶元素，直到：
            // 栈为空；已经删除了 k 位数字；新的栈顶元素不大于当前数字
            while (!queue.isEmpty() && count < k && queue.peekLast() > c) { // not "queue.peekLast() >= c"
                queue.pollLast();
                count++;
            }
            queue.offerLast(c);
        }
        // 上述过程结束后还需要针对一些情况做额外的处理：
        // 删除了 m 个数字并且 m < k，这种情况下需要从序列尾部删除额外的 k - m 个数字；
        // 最终的数字序列存在前导零，这种情况下需要删除前导零；
        // 最终的数字序列为空，这种情况下应该返回 0.
        // while (!queue.isEmpty() && count < k) { // num 的长度小于 10002 且 ≥ k
        while (count < k) {
            queue.pollLast();
            count++;
        }
        boolean leadingZero = true;
        StringBuilder sb = new StringBuilder();
        while (!queue.isEmpty()) {
            if (queue.peekFirst() == '0' && leadingZero) {
                queue.pollFirst();
            } else {
                leadingZero = false;
                sb.append(queue.pollFirst());
            }
        }
        return sb.length() == 0 ? "0" : sb.toString();
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 539. 最小时间差

> ※ 将时间全部转换为分钟制，然后对它们进行排序。**注**：不要忘记第一个时间和最后一个时间之间的差值！

```java
class Solution {
    public int findMinDifference(List<String> timePoints) {
        int n = timePoints.size();
        int[] times = new int[n];
        for (int i = 0; i < n; i++) {
            String time = timePoints.get(i);
            times[i] = (time.charAt(0) - '0') * 600 + (time.charAt(1) - '0') * 60 + (time.charAt(3) - '0') * 10 + time.charAt(4) - '0';
        }
        Arrays.sort(times);
        int res = 1440 - times[n - 1] + times[0];
        for (int i = 0; i < n - 1; i++) {
            res = Math.min(res, times[i + 1] - times[i]);
        }
        return res;
    }
}
```

> ※ 时间复杂度：取决于内部函数；空间复杂度：取决于内部函数。

#### 649. Dota2 参议院

> ※ 以天辉方为例，当一名天辉方的议员行使权力时，若当前所有的议员均为天辉方，那么该议员可以直接宣布天辉方取得胜利；若当前仍有夜魇方的议员，那么该议员只能行使“禁止一名参议员”的权利，并且他一定会挑选按照投票顺序的下一名夜魇方的议员（因为如果该议员挑选了其它投票较晚的议员，那么等到最早可以进行投票的那一名议员行使权力时，一名天辉方的议员就会丧失权力了）。综上所述，可以使用两个队列`r`、`d`分别按照投票顺序存储天辉方和夜魇方每一名议员的投票时间，并以此模拟整个投票过程：若`r`或`d`为空，那么可以宣布另一方获得胜利；若`r`或`d`均不为空，那么比较这两个队列的首元素，便可以确定当前行使权力的是哪一名议员。待行使完权力后，将该议员的投票时间加`n`并再次存入队列中。**注**：加`n`这一步操作很重要，可以防止没有议员能够阻止对方靠前的议员！

```java
class Solution {
    public String predictPartyVictory(String senate) {
        int n = senate.length();
        Queue<Integer> r = new LinkedList<Integer>();
        Queue<Integer> d = new LinkedList<Integer>();
        for (int i = 0; i < n; i++) {
            char c = senate.charAt(i);
            if (c == 'R') {
                r.add(i);
            } else {
                d.add(i);
            }
        }
        while (!r.isEmpty() && !d.isEmpty()) {
            int r_i = r.poll();
            int d_i = d.poll();
            if (r_i < d_i) {
                r.add(r_i + n); // 这里需要加 n ，以防止没人能禁止对方靠前的议员！
            } else {
                d.add(d_i + n);
            }
        }
        return d.isEmpty() ? "Radiant" : "Dire";
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 1324. 竖直打印单词

> ※ 首先，对字符串进行分词，并统计所有单词的最大长度；然后，对于结果链表中的第`i`个元素，依次遍历所有单词，若单词存在第`i`个字符，则将该字符加入元素的末尾，否则，将空格加入元素的末尾； 最后，借助特殊方法去掉元素末尾冗余的空格即可。

```java
class Solution {
    public List<String> printVertically(String s) {
        int wordMaxLen = 0;
        String[] words = s.split(" ");
        for (String word : words) {
            wordMaxLen = Math.max(wordMaxLen, word.length());
        }
        List<String> res = new ArrayList<String>();
        for (int i = 0; i < wordMaxLen; i++) {
            StringBuilder sb = new StringBuilder("!");
            for (String word : words) {
                char c = i >= word.length() ? ' ' : word.charAt(i);
                sb.append(c); // 原来 StringBuilder 可以直接 append 字符。
            }
            res.add(sb.toString().trim().substring(1)); // 是 substring 不是 subString !
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N max(|S|))$，其中，`N`表示单词的个数，`max(|S|)`表示最长的单词长度；空间复杂度：$O(N max(|S|))$。

#### 1451. 重新排列句子中的单词

> ※ 首先，利用`split(" ")`函数对字符串进行分割得到单词数组；接着，利用`toLowerCase()`函数将单词数组的第一个元素进行小写化；然后，利用`Arrays.sort()`函数对单词列表进行排序；最后，将排序后的第一个元素的首字母转换为大写，并对字符串列表进行拼接即可。

```java
class Solution {
    public String arrangeWords(String text) {
        String[] words = text.split(" ");
        words[0] = words[0].toLowerCase();
        Arrays.sort(words, (word1, word2) -> word1.length() - word2.length());
        words[0] = words[0].substring(0,1).toUpperCase() + words[0].substring(1);
        return String.join(" ", words);
    }
}
```

> ※ 时间复杂度：取决于内部函数；空间复杂度：取决于内部函数。

#### 1545. 找出第 N 个二进制字符串中的第 K 位

> ※ 详见<a href="https://leetcode-cn.com/problems/find-kth-bit-in-nth-binary-string/solution/zhao-chu-di-n-ge-er-jin-zhi-zi-fu-chuan-zhong-de-2/">解析</a>。

```java
class Solution {
    public char findKthBit(int n, int k) {
        if (k == 1) {
            return '0';
        }
        int m = 1 << (n - 1);
        if (k == m) {
            return '1';
        } else if (k < m) {
            return findKthBit(n - 1, k);
        } else {
            k = m * 2 - k;
            return invert(findKthBit(n - 1, k));
        }
    }
    private char invert(char bit) {
        return (char) ('0' + '1' - bit);
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 1647. 字符频次唯一的最小删除次数

> ※ 记录每个字符出现的次数并按从小到大的顺序排序，然后从倒数第二个元素开始遍历，

```java
class Solution {
    public int minDeletions(String s) {
        int res = 0;
        int[] charCnt = new int[26];
        for (char c : s.toCharArray()) {
            charCnt[c - 'a']++;
        }
        Arrays.sort(charCnt);
        for (int i = 24; i >= 0; i--) {
            while (charCnt[i] > 0 && charCnt[i] >= charCnt[i + 1]) {
                charCnt[i]--;
                res++;
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(NlogN)$；空间复杂度：$O(logN)$。

#### 1657. 确定两个字符串是否接近

> ※ 该题目共有四种情况：字符串长度不同，返回`false`；字符串含有的字符种类不同，返回`false`；排序后，含有的个数不同，返回`false`，其它均返回`true`。

```java
class Solution {
    public boolean closeStrings(String word1, String word2) {
        if (word1.length() != word2.length()) {
            return false;
        }
        int[] char1Cnt = new int[26];
        int[] char2Cnt = new int[26];
        for (char c : word1.toCharArray()) {
            char1Cnt[c - 'a']++;
        }
        for (char c : word2.toCharArray()) {
            char2Cnt[c - 'a']++;
        }
        for (int i = 0; i < 26; i++) {
            if (char1Cnt[i] != 0 && char2Cnt[i] == 0) {
                return false;
            }
            if (char1Cnt[i] == 0 && char2Cnt[i] != 0) {
                return false;
            }
        }
        Arrays.sort(char1Cnt);
        Arrays.sort(char2Cnt);
        for (int i = 0; i < 26; i++) {
            if (char1Cnt[i] != char2Cnt[i]) {
                return false;
            }
        }
        return true;
    }
}
```

> ※ 时间复杂度：$O(1)$；空间复杂度：$O(1)$。

-----