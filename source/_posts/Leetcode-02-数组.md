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

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

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

> ※ 时间复杂度：$O(NlogN)$；空间复杂度：取决于排序算法，若为堆排序，则为$O(1)$。

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

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

> ※ 考虑到只需要在满足下标约束（即下标之差的绝对值至多为`k`）的元素中进行查找，因此可以维护一个大小为`k`的`HashSet`，并始终在该`Set`中进行查找。由此，空间复杂度将降为$O(min(N, K))$。

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

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(min(N,K))$。

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

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 350. 两个数组的交集 II

> ※ 首先，遍历长度较小的数组，并将数组中每个数字以及它们出现的次数存储在`Map`中；然后，遍历另一个数组，对于另一个数组中的每个数字，若`Map`中存在该数字，则将该数字添加到答案，并减少`Map`中该数字出现的次数。

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return intersect(nums2, nums1);
        }
        int[] res = new int[nums1.length];
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int num : nums1) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        int index = 0;
        for (int i = 0; i < nums2.length; i++) {
            int count = map.getOrDefault(nums2[i], 0);
            if (count > 0) {
                res[index++] = nums2[i];
                map.put(nums2[i], count - 1);
            }
        }
        return Arrays.copyOfRange(res, 0, index); // 如何复制数组？
    }
}
```

> ※ 时间复杂度：$O(M + N)$；空间复杂度：$O(min(M, N))$。

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

> ※ 时间复杂度：$O(logN)$；空间复杂度：$O(1)$。

#### 485. 最大连续1的个数

> ※ 遍历数组，用`count`记录当前连续`1`的个数，`maxCount`记录当前最大连续`1`的个数。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int count = 0, maxCount = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 1) {
                count++;
            } else {
                maxCount = Math.max(count, maxCount);
                count = 0;
            }
        }
        return Math.max(count, maxCount);
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 496. 下一个更大元素 I

> ※ 最先想到的方法是：首先，借助`Map`保存`nums1`中的元素以及每个元素对应的下一个更大元素（初始时均为`-1`）；然后，遍历`nums2`，判断当前数字是否包含在`Map`中，若存在，则借助内部循环找到当前元素的下一个更大元素，若不存在，则遍历下一数字；最后，将`Map`的值返回即可。

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums1.length; i++) {
            map.put(nums1[i], -1);
        }
        for (int i = 0; i < nums2.length; i++) {
            if (map.containsKey(nums2[i])) {
                for (int j = i + 1; j < nums2.length; j++) {
                    if (nums2[j] > nums2[i]) {
                        map.put(nums2[i], nums2[j]);
                        break;
                    }
                }
            }
        }
        int[] res = new int[nums1.length];
        for (int i = 0; i < nums1.length; i++) {
            res[i] = map.get(nums1[i]);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(M + N)$，其中`M`、`N`分别表示`nums1`、`nums2`的长度；空间复杂度：$O(M)$。

> ※ 除此之外，题解中还给出了一种利用单调栈解决的方法，避免了内部循环所带来的重复遍历，按理说这种方法的时间复杂度应该要优于上一种方法，但事实证明，这种方法的运行时间更长。个人猜测可能与测试用例的设计有关。

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] res = new int[nums1.length];
        Stack<Integer> stack = new Stack<Integer>();
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums2.length; i++) {
            while (!stack.isEmpty() && nums2[i] > stack.peek()) {
                map.put(stack.pop(), nums2[i]);
            }
            stack.push(nums2[i]);
        }
        while (!stack.isEmpty()) {
            map.put(stack.pop(), -1);
        }
        for (int i = 0; i < nums1.length; i++) {
            res[i] = map.get(nums1[i]);
        }
        return res;
    }
}
```

※ 时间复杂度：$O(M + N)$，其中`M`、`N`分别表示`nums1`、`nums2`的长度；空间复杂度：$O(N)$。

#### 500. 键盘行

> ※ 将键盘上每一行的字母以集合的形式单独保存起来，然后将单词转换为集合，并判断其是否为某一行的子集。<span style="color:green">好久没用Python，都快忘了怎么用了。QAQ</span>

```python
class Solution:
    def findWords(self, words: List[str]) -> List[str]:
        res = []
        lines = [set('QWERTYUIOP'), set('ASDFGHJKL'), set('ZXCVBNM')]
        for word in words:
            word_set = set(word.upper())
            if word_set.issubset(lines[0]) or word_set.issubset(lines[1]) or word_set.issubset(lines[2]):
                res.append(word)
        return res
```

> ※ 时间复杂度：应该是$O(N)$吧，其中`N`表示所有单词包含的字符个数；空间复杂度：$O(1)$。

#### 506. 相对名次

> ※ 首先，对`nums`进行复制，得到`nums_clone`；然后，对`nums_clone`进行排序，并利用`Map`保存每个元素的排名；最后，基于每个元素的排名给出它的名次。

```java
class Solution {
    public String[] findRelativeRanks(int[] nums) {
        int[] nums_clone = nums.clone();
        Arrays.sort(nums_clone);
        String[] res = new String[nums.length];
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; i++) {
            map.put(nums_clone[i], i);
        }
        for (int i = 0; i < nums.length; i++) {
            res[i] = toString(nums.length - map.get(nums[i]));
        }
        return res;
    }
    private String toString(int rank) {
        if (rank == 1) {
            return "Gold Medal";
        } else if (rank == 2) {
            return "Silver Medal";
        } else if (rank == 3) {
            return "Bronze Medal";
        } else {
            return String.valueOf(rank);
        }
    }
}
```

> ※ 时间复杂度：$O(NlogN)$；空间复杂度：$O(N)$。

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

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

> **TODO**：利用矩阵求幂的方法，可以将时间复杂度优化至$O(logN)$；利用公式法，可以将时间复杂度和空间复杂度均优化至$O(1)$，后面有时间可以试一下。

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

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

> **TODO**：该题目同样可以利用`Morris`方法解决，后面有时间可以试一下。

#### 561. 数组拆分 I

> ※ 对数组进行排序，计算偶数下标对应的数字之和即可。

```java
class Solution {
    public int arrayPairSum(int[] nums) {
        Arrays.sort(nums);
        int res = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i % 2 == 0) {
                res += nums[i];
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(NlogN)$；空间复杂度：$O(1)$。

#### 682. 棒球比赛

> ※ 遍历字符串数组，若当前字符串为整数，则保存至栈中；否则，依据当前字符串对应的规则对栈进行操作。

```java
class Solution {
    public int calPoints(String[] ops) {
        int res = 0;
        Stack<Integer> stack = new Stack<Integer>();
        for (String op : ops) {
            if (op.equals("+")) {
                int top = stack.pop();
                int newtop = stack.peek() + top;
                stack.push(top);
                stack.push(newtop);
            } else if (op.equals("D")) {
                stack.push(2 * stack.peek());
            } else if (op.equals("C")) {
                stack.pop();
            } else {
                stack.push(Integer.valueOf(op));
            }
        }
        while (!stack.isEmpty()) {
            res += stack.pop();
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 704. 二分查找

> ※ 简单的二分查找，难点在于各种变形，如存在重复元素，数组进行了旋转，找到某个元素最先或最后出现的位置。

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
```

> ※ 时间复杂度：$O(logN)$；空间复杂度：$O(1)$。

#### 832. 翻转图像

> ※ 没有什么骚操作，按照题意一步一步来即可。需要注意的是，对于宽度为奇数的情况，不要忘记对其中间位置的数字进行反转。

```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        int rows = A.length, cols = A[0].length;
        for (int i = 0; i < rows; i++) {
            // for (int j = 0; j < cols / 2; j++) {
            //     int num = 1 - A[i][j];
            //     A[i][j] = 1 - A[i][cols - j - 1];
            //     A[i][cols - j - 1] = num;
            // }
            // if (cols % 2 == 1) {
            //     A[i][cols / 2] = 1 - A[i][cols / 2];
            // }
            for (int j = 0; j < (cols + 1) / 2; j++) {
                int num = 1 - A[i][j];
                A[i][j] = 1 - A[i][cols - j - 1];
                A[i][cols - j - 1] = num;
            }
        }
        return A;
    }
}
```

> ※ 时间复杂度：$O(MN)$；空间复杂度：$O(1)$。

#### 977. 有序数组的平方

> ※ 定义两个指针`i`、`j`，初始时分别指向数组的第`0`、`A.length - 1`个元素；比较两个指针所指元素的平方，选择较大的那个逆序放入结果数组中，并移动指针；重复上述过程，直至`i > j`。

```java
class Solution {
    public int[] sortedSquares(int[] A) {
        int[] res = new int[A.length];
        for (int i = 0, j = A.length - 1, pos = A.length - 1; i <= j; pos--) {
            int left = A[i] * A[i], right = A[j] * A[j];
            if (left > right) {
                res[pos] = left;
                i++;
            } else {
                res[pos] = right;
                j--;
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 1002. 查找常用字符

> ※ 依次遍历每个字符串，对字符串中每个字符出现的次数进行统计，最后，取每个字符出现次数的最小值即可。

```java
class Solution {
    public List<String> commonChars(String[] A) {
        int[] min_count = new int[26];
        Arrays.fill(min_count, Integer.MAX_VALUE); // Get 新技能
        for (int i = 0; i < A.length; i++) {
            int[] count = new int[26];
            for (int j = 0; j < A[i].length(); j++) {
                count[A[i].charAt(j) - 'a']++;
            }
            for (int j = 0; j < 26; j++) {
                min_count[j] = Math.min(min_count[j], count[j]);
            }
        }
        List<String> res = new LinkedList<String>();
        for (int i = 0; i < 26; i++) {
            for (int j = 0; j < min_count[i]; j++) {
                res.add(String.valueOf((char) ('a' + i)));
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(NM)$，其中，`N`表示字符串的个数，`M`表示字符串的平均长度；空间复杂度：$O(1)$。

#### 1184. 公交站间的距离

> ※ 按照题目意思，分别从前往后、从后往前对数组进行遍历并累加，取两次累加的最小值作为最终结果即可。

```java
class Solution {
    public int distanceBetweenBusStops(int[] distance, int start, int destination) {
        int forward = 0, backward = 0;
        for (int i = start; i != destination; i = (i + 1) % distance.length) {
            forward += distance[i];
        }
        for (int i = destination; i != start; i = (i + 1) % distance.length) {
            backward += distance[i];
        }
        return Math.min(forward, backward);
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 1431. 拥有最多糖果的孩子

> ※ 对于每一个孩子，只要其拥有的糖果数量加上额外的糖果数量大于所有孩子拥有的糖果数量的最大值，即可用于最多糖果。

```java
class Solution {
    public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
        int max = 0;
        for (int i = 0; i < candies.length; i++) {
            max = Math.max(max, candies[i]);
        }
        List<Boolean> res = new ArrayList<Boolean>();
        for (int i = 0; i < candies.length; i++) {
            // if (candies[i] + extraCandies >= max) {
            //     res.add(true);
            // } else {
            //     res.add(false);
            // }
            res.add(candies[i] + extraCandies >= max);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 1470. 重新排列数组

> ※ 该题目比较简单，按照题意一个一个遍历即可。

```java
class Solution {
    public int[] shuffle(int[] nums, int n) {
        int[] res = new int[2 * n];
        for (int i = 0; i < n; i++) {
            res[2 * i] = nums[i];
            res[2 * i + 1] = nums[i + n];
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

> **TODO**：该题目的空间复杂度最优可以降至`O(1)`，后面有时间可以试一下。

#### 1480. 一维数组的动态和

> ※ 该题目比较简单，利用一个临时变量，便可实现`O(1)`空间复杂度。

```java
class Solution {
    public int[] runningSum(int[] nums) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            nums[i] = sum;
        }
        return nums;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 1486. 数组异或操作

> ※ 按照题意，依次对数字进行异或操作即可。

```java
class Solution {
    public int xorOperation(int n, int start) {
        int res = 0;
        for (int i = start; i < start + 2 * n; i += 2) {
            res ^= i;
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

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

> ※ 时间复杂度：$O(MN)$；空间复杂度：$O(1)$。

> **注**：该题目尤其需要注意置零时的顺序，即首先对第一行之外的行进行置零，然后对所有列进行置零，最后对第一行进行置零！

#### 75. 颜色分类

> ※ 利用指针`P_0`来交换`0`，`p_2`来交换`2`。从左向右遍历数组，设当前遍历到的位置为`i`，对应的元素为`nums[i]`。若`nums[i] = 0`，则将其与`nums[p_0]`进行交换，并将`p_0`向后移动一个位置；若`nums[i] = 2`，则将其与`nums[p_2]`进行交换，并将`p_2`向前移动一个位置。需要注意的是，当我们找到`2`时，需要不断将其与`nums[p_2]`进行交换，直至新的`nums[i]`不为`2`。

```java
class Solution {
    public void sortColors(int[] nums) {
        int p_0 = 0, p_2 = nums.length - 1;
        for (int i = 0; i <= p_2; i++) { // 这里应取到等于。
            while (i <= p_2 && nums[i] == 2) {
                nums[i] = nums[p_2];
                nums[p_2--] = 2;
            }
            if (nums[i] == 0) {
                nums[i] = nums[p_0];
                nums[p_0++] = 0;
            }
        }
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

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

> ※ 时间复杂度：$O(logN)$；空间复杂度：$O(1)$。

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

> ※ 时间复杂度：$O(N)$，对于任意一个元素，最多只需要进行三次搜索，一次插入和一次删除，这些操作均为常量时间复杂度的；空间复杂度：$O(min(N,K))$，需要开辟的额外空间取决于`HashMap`的大小，而`HashMap`的大小的上限同时由`N`和`K`决定。

> **TODO**：该题目同样可以利用二叉搜索树解决，后面有时间可以试一下。

#### 238. 除自身以外数组的乘积

> ※ 初始化两个空数组`L`、`R`，对于`L`，其第`i`个元素表示`nums`中第`i`个元素之前所有数的乘积；对于`R`，其第`i`个元素表示`nums`中第`i`个元素之后所有数的乘积。借助上述思想，便可解决该题目。然而，上述思想的空间复杂度只有`O(N)`，达不到进阶的要求。为此，考虑将返回的结果数组视为`L`，通过一个常数维护`R`，从而达到`O(1)`空间复杂度。

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

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 289. 生命游戏

> ※ 该题目与`130. 被围绕的区域`类似，同样可以借助其它数字来表示“细胞”状态的更新，如`-1`表示细胞由活变死，`2`表示细胞由死变活。这里需要注意的是第`11`行代码，此时，`board[x][y] = -1`同样表示活细胞。

```java
class Solution {
    public void gameOfLife(int[][] board) {
        int rows = board.length, cols = board[0].length;
        int[][] directions = {{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                int count = 0;
                for (int[] direction : directions) {
                    int x = i + direction[0];
                    int y = j + direction[1];
                    if (-1 < x && x < rows && -1 < y && y < cols && (board[x][y] == 1 || board[x][y] == -1)) {
                        count++;
                    }
                }
                if (board[i][j] == 0 && count == 3) {
                    board[i][j] = 2;
                } else if (board[i][j] == 1 && (count < 2 || count > 3)) {
                    board[i][j] = -1;
                }
            }
        }
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (board[i][j] == -1) {
                    board[i][j] = 0;
                } else if (board[i][j] == 2) {
                    board[i][j] = 1;
                }
            }
        }
    }
}
```

> ※ 时间复杂度：$O(MN)$；空间复杂度：$O(1)$。

#### 419. 甲板上的战舰

> ※ 完全没有思路，看了<a href="https://leetcode-cn.com/problems/battleships-in-a-board/solution/">题解</a>之后才会做，我真的是好菜啊。对二维矩阵进行扫描，扫描到`X`时，如果其上方或左方也是`X`，则不计数，否则计数加`1`。

```java
class Solution {
    public int countBattleships(char[][] board) {
        int res = 0;
        for (int i = 0; i< board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == 'X') {
                    if (i > 0 && board[i - 1][j] == 'X') {
                        continue;
                    }
                    if (j > 0 && board[i][j - 1] == 'X') {
                        continue;
                    }
                    res++;
                }
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(MN)$；空间复杂度：$O(1)$。

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

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 739. 每日温度

> ※ 该题目与`496. 下一个更大元素 I`类似，同样利用了“单调栈”的思想，只是这里将元素的下标保存至栈中，而非元素本身。

```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] res = new int[T.length];
        Stack<Integer> stack = new Stack<Integer>();
        for (int i = 0; i < T.length; i++) {
            while (!stack.isEmpty() && T[i] > T[stack.peek()]) {
                int pos = stack.pop();
                res[pos] = i - pos;
            }
            stack.push(i);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 845. 数组中的最长山脉

> ※ 定义指针`l`指向左侧山脚，初始值为`0`。当每次固定完`l`后，首先，需要保证`l + 2 < n`，因为山脉的长度至少为`3`；其次，需要保证`A[l] < A[l + 1]`，否则`l`对应的不可能为左侧山脚；然后，将指向右侧山脚的指针`r`初始值置为`l + 1`，并不断向右移动`r`，直至`A[r] < A[r + 1]`不满足，此时：若`r = n - 1`，说明已经移至了数组末尾，无法形成山脉；否则，`r`对应的可能为山顶，还需要额外判定是否有`A[r] > A[i + 1]`，若是则说明`r`对应的为山顶，此时应采用类似的方法，不断向右移动`r`，直至`A[r] > A[r + 1]`不满足，`r - l + 1`即对应着山脉的长度。

```java
class Solution {
    public int longestMountain(int[] A) {
        int res = 0;
        int l = 0, n = A.length;
        while (l + 2 < n) {
            int r = l + 1;
            if (A[l] < A[r]) {
                while (r + 1 < n && A[r] < A[r + 1]) {
                    r++;
                }
                if (r + 1 < n && A[r] > A[r + 1]) {
                    while (r + 1 < n && A[r] > A[r + 1]) {
                        r++;
                    }
                    res = Math.max(res, r - l + 1);
                } else {
                    r++;
                }
            }
            l = r;
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 1004. 最大连续1的个数 III

> ※ 该题目可以理解为：滑动窗口内最多有`K`个`0`，求滑动窗口的最大长度。

```java
class Solution {
    public int longestOnes(int[] A, int K) {
        int count = 0, maxCount = 0;
        int left = 0, right = 0;
        // 滑动窗口表示的区间为[left, right)，左闭右开
        while (right < A.length) {
            // 窗口扩充一个元素，若为 0 则 count++；
            if (A[right++] == 0) {
                count++;
            }
            // 当窗口内 0 的个数超过 K 时，开始收缩窗口。
            while (count > K) {
                // 若滑出窗口的元素是 0，则 count--；
                if (A[left++] == 0) {
                    count--;
                }
            }
            // 此时 count <= K，保存窗口的最大宽度；
            maxCount = Math.max(maxCount, right - left);
        }
        return maxCount;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 1630. 等差子数组

> ※ 依次拷贝每一个子数组，并对子数组进行排序，然后依次判断子数组中的每个元素是否满足等差数列的性质`2 * A[i] == A[i + 1] + A[i - 1]`即可。**注**：当子数组的长度小于`3`时必为等差数组。

```java
class Solution {
    public List<Boolean> checkArithmeticSubarrays(int[] nums, int[] l, int[] r) {
        int len = l.length;
        List<Boolean> res = new LinkedList<Boolean>();
        for (int i = 0; i < len; i++) {
            int[] subArray = Arrays.copyOfRange(nums, l[i], r[i] + 1);
            Arrays.sort(subArray);
            if (subArray.length < 3) {
                res.add(true);
            } else {
                boolean sign = true;
                for (int j = 1; j <subArray.length - 1; j++) {
                    if (2 * subArray[j] != subArray[j - 1] + subArray[j + 1]) {
                        sign = false;
                        break;
                    }
                }
                res.add(sign);
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：待定；空间复杂度：待定。

#### 1631. 最小体力消耗路径

> ※ 假设当前最短路径的长度为`limit`，对二维数组进行广度优先搜索，并且只扩展“长度”小于等于`limit`的边，若能够扩展到目的节点，则缩小`limit`，否则扩大`limit`。

```java
class Solution {
    private int[][] directions = new int[][] {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    public int minimumEffortPath(int[][] heights) {
        int l = 0, r = 1000000;
        int rows = heights.length, cols = heights[0].length;
        while (l <= r) {
            int m = l + (r - l) / 2;
            Deque<int[]> queue = new LinkedList<int[]>();
            boolean[][] visited = new boolean[rows][cols];
            visited[0][0] = true;
            queue.add(new int[]{0, 0});
            while (!queue.isEmpty()) {
                int[] pos = queue.poll();
                int x = pos[0], y = pos[1];
                for (int[] direction : directions) {
                    int nx = x + direction[0];
                    int ny = y + direction[1];
                    if (-1 < nx && nx < rows && -1 < ny && ny < cols && !visited[nx][ny]) {
                        if (Math.abs(heights[nx][ny] - heights[x][y]) <= m) {
                            visited[nx][ny] = true;
                            queue.offer(new int[]{nx, ny});
                        }
                        if (visited[rows - 1][cols - 1]) {
                            break;
                        }
                    }
                }
            }
            if (visited[rows - 1][cols - 1]) {
                r = m - 1;
            } else {
                l = m + 1;
            }
        }
        return l;
    }
}
```

> ※ 时间复杂度：待定；空间复杂度：待定。

-----