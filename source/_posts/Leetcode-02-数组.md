---
title: 02 数组
date: 2020-09-24 11:48:46
categories:
- Leetcode
tags:
- 数组
---

### 简单

-----

#### 169. 多数元素

> ※ 对数组进行排序，下标为`n/2`的元素一定为众数。

```java
class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length / 2];
    }
}
```

> ※ 时间复杂度：$O(NlogN)$；空间复杂度：$O(logN)$。

#### 217. 存在重复元素

> ※ 对数组中的元素进行遍历，并将遍历过的元素保存至`HashSet`中。对于某个新访问的元素，判断其是否存在于`HashSet`中，若存在，则说明数组中存在重复元素；若不存在，则将其保存至`HashSet`中，并继续进行遍历，直至遍历完所有元素。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        for(int num : nums) {
            if (set.contains(num)) {
                return true;
            } else {
                set.add(num);
            }
        }
        return false;
    }
}
```

> ※ 时间复杂度：$O(n)$；空间复杂度：$O(n)$。

> ※ 对数组进行排序，若数组中存在重复元素，则重复元素一定相邻。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0;i < nums.length - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
}
```

> ※ 时间复杂度：$O(nlogn)$；空间复杂度：取决于排序算法，若为堆排序，则为$O(1)$。

> **注**：由<a href="https://leetcode-cn.com/problems/contains-duplicate/solution/cun-zai-zhong-fu-yuan-su-by-leetcode/">题解</a>得知，对于一些特定的`n`不太大的测试样例，方法一的运行速度可能会比方法二慢，这是因为哈希表在维护其属性时，需要一些额外的开销。
>
> **注**：方法二对原数组进行了修改，通常情况下，除非调用方清楚输入数据会被修改，否则不应该随意修改输入数据。为此，可以先复制`nums`，然后对副本进行操作。

#### 219. 存在重复元素 II

> ※ 对数组中的元素进行遍历，并将遍历过的元素及其下标保存至`HashMap`中。对于某个新访问的元素，判断其是否存在于`HashSet`中，并且满足下标之差的绝对值至多为`k`，若存在，则说明数组中存在满足约束的重复元素；若不存在，则将其及其下标保存至`HashMap`中，并继续进行遍历，直至遍历完所有元素。

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i  = 0; i < nums.length; i++) {
            if (map.get(nums[i]) != null && i - map.get(nums[i]) <= k) {
                return true;
            } else {
                map.put(nums[i], i);
            }
        }
        return false;
    }
}
```

> ※ 时间复杂度：$O(n)$；空间复杂度：$O(n)$。

> ※ 考虑到只需要在满足下标约束（即下标之差的绝对值至多为`k`）的元素中进行查找，因此可以维护一个大小为`k`的`HashSet`，并始终在该`Set`中进行查找。由此，空间复杂度将降为$O(min(n, k))$。

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet<Integer>();
        for (int i = 0; i < nums.length; i++) {
            if (set.contains(nums[i])) {
                return true;
            } else {
                set.add(nums[i]);
            }
            if (set.size() > k) {
                set.remove(nums[i - k]);
            }
        }
        return false;
    }
}
```

> ※ 时间复杂度：$O(n)$；空间复杂度：$O(min(n, k))$。

#### 283. 移动零

> ※ 借助快慢指针的思想，将快指针指向当前元素，慢指针指向可移动位置，快指针与慢指针之间的所有元素均为零。若当前元素不为零，则交换快慢指针所指元素。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        for (int i = 0, j = 0; j < nums.length; j++) {
            if (nums[j] != 0) {
                int num = nums[j];
                nums[j] = nums[i];
                nums[i++] = num;
            }
        }
    }
}
```

> ※ 时间复杂度：$O(n)$；空间复杂度：$O(1)$。

#### 374. 猜数字大小

> ※ 首先能想到的方法便是依次遍历所有数字，但是这种方法的时间复杂度较高。若借助二分查找的思想，能够将时间复杂度优化至$O(logn)$。

```java
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int min = 1, max = n;
        int mid = min + (max - min) / 2;
        while (guess(mid) != 0) {
            if (guess(mid) == 1) {
                min = mid + 1;
            } else {
                max = mid - 1;
            }
            mid = min + (max - min) / 2;
        }
        return mid;
    }
}
```

> ※ 时间复杂度：$O(logn)$；空间复杂度：$O(1)$。

#### 509. 斐波那契数

> ※ 比较经典的题目，一般情况下都能够将时间复杂度和空间复杂度分别优化至$O(n)$和$O(1)$。

```java
class Solution {
    public int fib(int N) {
        if (N == 0 || N == 1) {
            return N;
        } else {
            int pre1 = 0, pre2 = 1;
            for (int i = 1; i < N; i++) {
                int cur = pre1 + pre2;
                pre1 = pre2;
                pre2 = cur;
            }
            return pre2;
        }
    }
}
```

> ※ 时间复杂度：$O(n)$；空间复杂度：$O(1)$。

> **TODO**：利用矩阵求幂的方法，可以将时间复杂度优化至$O(logn)$；利用公式法，可以将时间复杂度和空间复杂度均优化至$O(1)$，后面有时间可以试一下。

#### 538. 把二叉搜索树转换为累加树

> ※ 对于二叉搜索树，其中序遍历是递增的；反过来，若先访问右子树，再访问根节点，最后访问左子树，则遍历是递减的（这正是我们所需要的）。基于此，只需反向中序遍历二叉搜索树，记录遍历过程中的节点值之和，并不断更新当前遍历节点的节点值，即可得到累加树。

```java
class Solution {
    private int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if (root != null) {
            convertBST(root.right);
            sum += root.val;
            root.val = sum;
            convertBST(root.left);
        }
        return root;
    }
}
```

> ※ 时间复杂度：$O(n)$；空间复杂度：$O(n)$。

> **TODO**：该题目同样可以利用`Morris`方法解决，后面有时间可以试一下。

-----

### 中等

-----

#### 73. 矩阵置零

> ※ 将每行和每列的第一个元素视为标记，用于表示该行或该列是否需要置零。对于第一行元素，可以额外声明一个变量，用于表示第一行是否需要置零。

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        if (matrix.length == 0) {
            return;
        }
        // 判断第一行是否需要置零
        boolean setZero = false;
        for (int i = 0; i < matrix[0].length; i++) {
            if (matrix[0][i] == 0) {
                setZero = true;
                break;
            }
        }
        // 判断第一列是否需要置零
        for (int i = 0; i < matrix.length; i++) {
            if (matrix[i][0] == 0) {
                matrix[0][0] = 0;
                break;
            }
        }
        //
        for (int i = 1; i < matrix.length; i++) {
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        //
        for (int i = 1; i < matrix.length; i++) {
            if (matrix[i][0] == 0) {
                for (int j = 1; j < matrix[0].length; j++) {
                    matrix[i][j] = 0;
                }
            }
        }
        //
        for (int i = 0; i < matrix[0].length; i++) {
            if (matrix[0][i] == 0) {
                for (int j = 1; j < matrix.length; j++) {
                    matrix[j][i] = 0;
                }
            }
        }
        //
        if (setZero) {
            for (int i = 0; i < matrix[0].length; i++) {
                matrix[0][i] = 0;
            }
        }
        // MD，我是真的菜，看完解析做了三遍才做出来。。。
    }
}
```

> ※ 时间复杂度：$O(mn)$；空间复杂度：$O(1)$。

> **注**：该题目尤其需要注意置零时的顺序，即首先对第一行之外的行进行置零，然后对所有列进行置零，最后对第一行进行置零！

#### 153. 寻找旋转排序数组中的最小值

> ※ 首先能够想到的便是利用二分搜索，问题在于如何更新状态。对于`left`、`mid`、`right`，它们之间的关系存在如下四种可能：`< <`、`< >`、`> <`、`> >`，分析出每种可能对应的状态更新，便能够轻松解决该题目。更详细的分析请参考<a href="https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/solution/er-fen-cha-zhao-wei-shi-yao-zuo-you-bu-dui-cheng-z/">题解</a>。

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left <= right) { // 能否取等号取决于 mid 能否等于 right
            int mid = left + (right - left) / 2;
            if (nums[mid] < nums[right]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        } 
        return nums[right];
    }
}
```

> ※ 时间复杂度：$O(logn)$；空间复杂度：$O(1)$。

#### 220. 存在重复元素 III

> ※ 基于桶排序的思想，将窗口当作桶来实现一个线性复杂度的解法。

```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        Map<Long, Long> map = new HashMap<Long, Long>(); // 注意需要用到 Long 类型，防止溢出。
        for (int i = 0; i < nums.length; i++) {
            long id = getBucketId(nums[i], t + 1); // t + 1 !!!
            if (map.containsKey(id)) {
                return true;
            } else if (map.containsKey(id - 1) && Math.abs(map.get(id - 1) - nums[i]) <= t) {
                return true;
            } else if (map.containsKey(id + 1) && Math.abs(map.get(id + 1) - nums[i]) <= t) {
                return true;
            }
            map.put(id, (long) nums[i]);
            if (map.size() > k) {
                map.remove(getBucketId(nums[i - k], t + 1));
            }
        }
        return false;
    }

    private long getBucketId(long num, long t) {
        return num >= 0 ? num / t : (num + 1) / t - 1; // 注意判断条件为 num >= 0 而非 num > 0
    }
}
```

> ※ 时间复杂度：$O(n)$，对于任意一个元素，最多只需要进行三次搜索，一次插入和一次删除，这些操作均为常量时间复杂度的；空间复杂度：$O(min(n, k))$，需要开辟的额外空间取决于`HashMap`的大小，而`HashMap`的大小的上限同时由`n`和`k`决定。

> **TODO**：该题目同样可以利用二叉搜索树解决，后面有时间可以试一下。

#### 238. 除自身以外数组的乘积

> ※ 初始化两个空数组`L`、`R`，对于`L`，其第`i`个元素表示`nums`中第`i`个元素之前所有数的乘积；对于`R`，其第`i`个元素表示`nums`中第`i`个元素之后所有数的乘积。借助上述思想，便可解决该题目。然而，上述思想的空间复杂度只有`O(n)`，达不到进阶的要求。为此，考虑将返回的结果数组视为`L`，通过一个常数维护`R`，从而达到`O(1)`空间复杂度。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] res = new int[nums.length];
        res[0] = 1;
        for (int i = 1; i < nums.length; i++) {
            res[i] = res[i - 1] * nums[i - 1];
        }
        int temp = 1;
        for (int i = nums.length - 1; i > -1; i--) {
            res[i] = res[i] * temp;
            temp *= nums[i];
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(n)$；空间复杂度：$O(1)$。

#### 670. 最大交换

> ※ 自右向左统计大于（不包含等于）当前字符的最大字符的下标；然后，自左向右遍历字符，若当前字符的下标不等于大于其最大字符的下标，并且这两个字符不相等（该约束很重要！），则对它们进行交换并退出。

```java
class Solution {
    public int maximumSwap(int num) {
        char[] nums = String.valueOf(num).toCharArray();
        char max_c = '0';
        int max_i = nums.length - 1;
        int[] arr = new int[nums.length];
        arr[max_i] = max_i;
        for (int i = nums.length - 1; i > -1; i--) {
            if (nums[i] > max_c) {
                max_c = nums[i];
                max_i = i;
            }
            arr[i] = max_i;
        }
        for (int i = 0; i < nums.length; i++) {
            if (arr[i] != i && nums[arr[i]] != nums[i]) {
                char c = nums[arr[i]];
                nums[arr[i]] = nums[i];
                nums[i] = c;
                break;
            }
        }
        return Integer.parseInt(String.valueOf(nums));
    }
}
```

> ※ 时间复杂度：$O(n)$；空间复杂度：$O(n)$。

-----