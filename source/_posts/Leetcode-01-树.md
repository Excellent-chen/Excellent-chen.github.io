---
title: 01 树
date: 2020-09-24 11:46:30
categories:
- Leetcode
tags:
- 树
---

<!-- more -->

### 简单

-----

#### 108. 将有序数组转换为二叉搜索树

> ※ 选择数组中间位置的数字作为二叉搜索树的根节点，则分给左右子树的数字个数相等或只相差`1`，由此可以使得二叉搜索树保持平衡。

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return buildBST(nums, 0, nums.length - 1);
    }

    private TreeNode buildBST(int[] nums, int left, int right) {
        if (left <= right) {
            int mid = left + (right - left) / 2;
            TreeNode node = new TreeNode(nums[mid]);
            node.left = buildBST(nums, left, mid - 1);
            node.right = buildBST(nums, mid + 1, right);
            return node;
        } else {
            return null;
        }
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(logN)$。

#### 110. 平衡二叉树

> ※ 自底向上遍历该平衡二叉树，若某颗子树不是平衡二叉树，那么该二叉树便不是平衡二叉树。

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) >= 0;
    }

    private int height(TreeNode node) {
        if (node == null) {
            return 0;
        }
        int l = height(node.left);
        int r = height(node.right);
        if (l == -1 || r == -1 || Math.abs(l - r) > 1) {
            return -1;
        } else {
            return Math.max(l, r) + 1;
        }
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 235. 二叉搜索树的最近公共祖先

> ※ `p`、`root`、`q`之间共有可能存在`9`种大小关系，弄清每种大小关系出现时，如何进行下一步判断，就能够轻松解决该题目。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root != null && p != null && q != null) {
            if (p.val > root.val && root.val < q.val) {
                return lowestCommonAncestor(root.right, p, q);
            } else if (p.val < root.val && root.val > q.val) {
                return lowestCommonAncestor(root.left, p, q);
            } else {
                return root;
            }
        } else {
            return null;
        }
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 501. 二叉搜索树中的众数

> ※ 想了一段时间后实在想不出来，就按照官方题解实现了一下。狗屎，时间复杂度和空间复杂度完全比不上利用递归的时间复杂度和空间复杂度。好在借此机会了解了一下真·$O(1)$空间复杂度的`Morris`遍历方法，如果有题目规定不能使用额外的空间，包括由递归产生的隐式调用栈的开销，那么该方法就派的上用场了。另外，在此分享一篇关于介绍`Morris`遍历方法的不错的<a href="https://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html">博客</a>，后面有时间可以仔细看一看。

```java
class Solution {
    private int base, count, maxCount;
    private List<Integer> res = new ArrayList<Integer>();
    public int[] findMode(TreeNode root) {
        TreeNode node = root;
        while (node != null) {
            if (node.left == null) {
                // 若当前节点的左孩子为空，则输出当前节点，并将当前节点更新为它的右孩子。
                counter(node.val);
                node = node.right;
            } else {
                TreeNode preNode = node.left;
                // 若当前节点的左孩子不为空，则在其左子树中找到其前驱节点。
                while (preNode.right != null && preNode.right != node) {
                    preNode = preNode.right;
                }
                if (preNode.right == null) {
                    // 若前驱节点的右孩子为空，则将其右孩子设置为当前节点，
                    // 并将当前节点更新为当前节点的左孩子。
                    preNode.right = node;
                    node = node.left;
                } else {
                    // 若前驱节点的右孩子为当前节点，则将其右孩子重新设为空（恢复树的形状），
                    // 输出当前节点，并将当前节点更新为当前节点的右孩子。
                    preNode.right = null;
                    counter(node.val);
                    node = node.right;
                }
            }
        }
        int[] mode = new int[res.size()];
        for (int i = 0; i < res.size(); i++) {
            mode[i] = res.get(i);
        }
        return mode;
    }
    private void counter(int val) {
        if (val == base) {
            count++;
        } else {
            count = 1;
            base = val;
        }
        if (count == maxCount) {
            res.add(val);
        } else if (count > maxCount) {
            maxCount = count;
            res.clear();
            res.add(val);
        }
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

> **TODO**：该题目还可以利用递归解决，并且运行效果更好，后面有时间可以试一下。

#### 530. 二叉搜索树的最小绝对差

> ※ 二叉搜索树有一个很重要的性质：其中序遍历得到的序列是非单调递减的。基于该性质，该题目一种常见的解法是：首先，对二叉搜索树进行中序遍历，并将节点值保存至数组中；然后，对数组进行遍历，保存相邻元素之差的绝对值；最后，从所有相邻元素之差的绝对值中取最小值。然而，为了保存节点值，我们需要使用额外的空间，能否在遍历过程中直接获取相邻元素呢？答案是肯定的（见代码）。

```java
class Solution {
    private int pre = -1, res = Integer.MAX_VALUE;
    public int getMinimumDifference(TreeNode root) {
        middleOrderTraversal(root);
        return res;
    }
    private void middleOrderTraversal(TreeNode node) {
        if (node == null) {
            return;
        }
        middleOrderTraversal(node.left);
        // 这两个条件语句避免了额外引入数组保存节点值！
        if (pre == -1) {
            pre = node.val;
        } else {
            res = Math.min(res, Math.abs(node.val - pre));
            pre = node.val;
        }
        middleOrderTraversal(node.right);
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 617. 合并二叉树

> ※ 两个二叉树中对应的节点存在以下几种情况：一个节点为空，另一个节点不为空，此时合并后的节点应该为不为空的那个节点；两个节点均不为空，此时合并后的节点为其中任意一个节点即可，但需要对返回的节点的值进行修改；两个节点均为空，此时合并后的节点为其中任意一个节点即可，反正都为空。

```java
class Solution {
    private TreeNode root;
    private TreeNode node = root;
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null) {
            
        }
        if (t1 != null && t2 != null) {
            node.val = t1.val + t2.val;
        } else if (t1 == null && t2 != null) {
            node.val = t2.val;
        } else if (t1 != null && t2 == null) {
            node.val = t1.val;
        }
    }
}
```

> ※ 时间复杂度：$O(M,N)$，其中`M`、`N`分别表示两个二叉树的节点数，只有当两个二叉树中对应的节点均不为空时，才会对两个节点进行显性合并操作，因此被访问到的节点数不会超过较小的二叉树的节点数；空间复杂度：$O(M,N)$，空间复杂度取决于递归调用的层数，而递归调用的层数不会超过较小的二叉树的节点数。

#### 700. 二叉搜索树中的搜索

> ※ 该题目比较简单，依据二叉搜索树的性质，递归进行求解即可。

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null || root.val == val) {
            return root;
        } else if (root.val > val) {
            return searchBST(root.left, val);
        } else {
            return searchBST(root.right, val);
        }
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

-----

### 中等

-----

#### 103. 二叉树的锯齿形层次遍历

> ※ 借助双端队列和一个用于指定方向的临时变量即可解决。

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<List<Integer>>();
        if (root == null) {
            return res;
        }
        int left_to_right = 0;
        Deque<TreeNode> deque = new LinkedList<TreeNode>();
        deque.offerFirst(root);
        while(deque.size() > 0) {
            int size = deque.size();
            List<Integer> list = new LinkedList<Integer>();
            while(size-- > 0) {
                if (left_to_right == 0) {
                    TreeNode node = deque.pollLast();
                    if (node.left != null) {
                        deque.offerFirst(node.left);
                    }
                    if (node.right != null) {
                        deque.offerFirst(node.right);
                    }
                    list.add(node.val);
                } else {
                    TreeNode node = deque.pollFirst();
                    if (node.right != null) {
                        deque.offerLast(node.right);
                    }
                    if (node.left != null) {
                        deque.offerLast(node.left);
                    }
                    list.add(node.val);
                }
            }
            left_to_right = 1 - left_to_right;
            res.add(list);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 105. 从前序与中序遍历序列构造二叉树

> ※ 对于前序遍历来说，最先遍历的节点一定为根节点；借助中序遍历，可以分别得知根节点左右子树包含节点的数量，从而进一步获取它们的先序遍历和中序遍历。以此类推，利用递归便可解决该题目。

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int len = preorder.length;
        if (len == 0) {
            return null;
        }
        int val = preorder[0];
        TreeNode node = new TreeNode(val);
        int index;
        for (index = 0; index < len - 1; index++) {
            if (inorder[index] == val) {
                break;
            }
        }
        node.left = buildTree(Arrays.copyOfRange(preorder, 1, index + 1), Arrays.copyOfRange(inorder, 0, index));
        node.right = buildTree(Arrays.copyOfRange(preorder, index + 1, len), Arrays.copyOfRange(inorder, index + 1, len));
        return node;
    }
}
```

> ※ 时间复杂度：$O(M)$；空间复杂度：$O(N)$。

> **注**：在获取`index`以及对数组进行复制`Arrays.copyOfRange()`时，涉及到了大量的遍历，这些遍历其实是可以通过增加形参省略的，你知道怎么做嘛？

#### 106. 从中序与后序遍历序列构造二叉树

> ※ 对于后序遍历来说，最后遍历的节点一定为根节点；借助中序遍历，可以分别得知根节点左右子树包含节点的数量，从而进一步获取它们的中序遍历和后序遍历。以此类推，利用递归便可解决该题目。

```java
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int len = postorder.length;
        if (len == 0) {
            return null;
        }
        int val = postorder[len - 1];
        TreeNode node = new TreeNode(val);
        int index;
        for (index = 0; index < len - 1; index++) {
            if (inorder[index] == val) {
                break;
            }
        }
        node.left = buildTree(Arrays.copyOfRange(inorder, 0, index), Arrays.copyOfRange(postorder, 0, index));
        node.right = buildTree(Arrays.copyOfRange(inorder, index + 1, len), Arrays.copyOfRange(postorder, index, len - 1));
        return node;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

> **注**：同样的，在获取`index`以及对数组进行复制`Arrays.copyOfRange()`时，涉及到了大量的遍历，这些遍历其实是可以通过增加形参省略的，你知道怎么做嘛？

#### 109. 有序链表转换二叉搜索树

> ※ 该题目与`108. 将有序数组转换为二叉搜索树`类似，只是将有序数组换为了有序链表。在解决该题目时，首先可以利用快慢指针找到链表的中间节点，并以该中间节点作为当前二叉搜索树的根节点，然后分别以中间节点两侧的链表递归构建二叉搜索树，作为该根节点的左子树和右子树。

```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        return buildBST(head);
    }
    private TreeNode buildBST(ListNode node) {
        if (node == null) {
            return null;
        } else if (node.next == null) {
            // 对应链表只包含一个节点的情况
            return new TreeNode(node.val);
        } else {
            // 对应链表包含两个或两个以上节点的情况
            ListNode pre = null;
            ListNode slow = node, fast = node;
            while (fast != null && fast.next != null) {
                pre = slow;
                slow = slow.next;
                fast = fast.next.next;
            }
            TreeNode root = new TreeNode(slow.val);
            pre.next = null;
            // 若链表中只包含两个节点，则 pre 指向 node，即第一个节点；
            // slow 指向第二个节点，fast 指向 null。
            // 此时，以 slow 作为根节点，node 作为左孩子节点，null作为右孩子节点是合理的。
            root.left = buildBST(node);
            root.right = buildBST(slow.next);
            return root;
        }
    }
}
```

> ※ 时间复杂度：$O(NlogN)$；空间复杂度：$O(logN)$，递归过程中栈的最大深度，也即平衡二叉树的高度。

> **TODO**： <a href="https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/solution/you-xu-lian-biao-zhuan-huan-er-cha-sou-suo-shu-1-3/">题解</a>中还给出了一种分治加中序遍历优化的方法，在空间复杂度保持`O(logN)`的前提下，时间复杂度只有`O(N)`。此外，该题目与上一题均是将中间节点作为了根节点，那么按照该种方法构造出的二叉树一定是平衡的吗？答案是肯定的，但是你知道要怎么证明吗？这些题解里都有，后面有时间可以看一下。

#### 116. 填充每个节点的下一个右侧节点指针

> ※ 该题目与`117. 填充每个节点的下一个右侧节点指针 II`的区别在于，指定了二叉树为完美二叉树。由完美二叉树的性质可以得知：只要每一层第一个节点的左孩子不为空，那么下一层的节点就一定都存在。利用该性质，便可解决该题目。

```java
class Solution {
    public Node connect(Node root) {
        if (root == null) {
            return null;
        }
        Node start = root;
        while (start.left != null) { // 只要 start.left 不为零，就能保证下一层节点都存在
            Node nextStart = start.left;
            for (Node node = start; node != null; node = node.next) {
                node.left.next = node.right;
                if (node.next != null) {
                    node.right.next = node.next.left;
                }
            }
            start = nextStart;
        }
        return root;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 117. 填充每个节点的下一个右侧节点指针 II

> ※ 在访问第`i`层时，建立第`i+1`层节点的`next`指针，便能够将时间复杂度由层次遍历的`O(n)`降至`O(1)`。

```java
class Solution {
    private Node nextStart = null, nextEnd = null;
    public Node connect(Node root) {
        if (root == null) {
            return null;
        }
        Node start = root;
        while (start != null) {
            nextStart = null;
            nextEnd = null;
            for (Node node = start; node != null; node = node.next) {
                if (node.left != null) {
                    helper(node.left);
                }
                if (node.right != null) {
                    helper(node.right);
                }
            }
            start = nextStart;
        }
        return root;
    }
    private void helper(Node node) {
        if (nextStart == null) {
            nextStart = node;
        }
        if (nextEnd != null) {
            nextEnd.next = node;
        }
        nextEnd = node;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(1)$。

#### 199. 二叉树的右视图

> ※ 对二叉树进行从右往左的层次遍历，遍历每一层节点之前，先保留当前层最右边的节点的值。

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new LinkedList<Integer>();
        if (root == null) {
            return res;
        }
        Deque<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int count = queue.size();
            res.add(queue.peek().val);
            while (count > 0) {
                count--;
                TreeNode node = queue.poll();
                if (node.right != null) {
                    queue.offer(node.right);
                }
                if (node.left != null) {
                    queue.offer(node.left);
                }
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 222. 完全二叉树的节点个数

> ※ 该题目的关键在于计算二叉树的深度以及最底层叶子节点的个数。对于前者，可以对二叉树进行深度优先搜索，并保存每个节点对应的深度，**最先**遇到的满足`root.left == null && root.right == null`的节点对应的深度即为该二叉树的深度；对于后者，可以借助上一步中求出的`depth`，在对二叉树进行深度优先搜索的同时获取。

```java
class Solution {

    private boolean setDepth = false;

    private int count = 0, depth = 0;

    public int countNodes(TreeNode root) {
        DFS(root, 0);
        return (int)(Math.pow(2, this.depth) + this.count - 1);
    }

    private void DFS(TreeNode root, int depth) {
        if (root == null) {
            return;
        } else if (root.left == null && root.right == null) {
            if (this.depth == 0 && !this.setDepth) {
                this.setDepth = true;
                this.depth = depth;
            }
            if (this.depth == depth) {
                count++;
            }
        } else {
            DFS(root.left, depth + 1);
            DFS(root.right, depth + 1);
        }
    }

}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 230. 二叉搜索树中第K小的元素

> ※ 二叉搜索树中第`K`小的元素对应了二叉搜索树中序遍历的第`K`个元素，因此，只需对二叉搜索树进行中序遍历，并对遍历过的节点进行计数，待遍历到第`K`个节点时，返回该节点的值即可。

```java
class Solution {
    private int key = 0, res = 0;
    public int kthSmallest(TreeNode root, int k) {
        inOrderTraversal(root, k);
        return res;
    }
    private void inOrderTraversal(TreeNode root, int k) {
        if (root == null) {
            return;
        }
        inOrderTraversal(root.left, k);
        key++;
        // if (key == k) {
        //     res = root.val;
        //     return;
        // }
        if (key == k) {
            res = root.val;
        } else if (key > k) {
            return;
        }
        inOrderTraversal(root.right, k);
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

> **TODO**：第`34`行引入的条件语句能够缩短递归执行时间吗？

#### 429. N叉树的层序遍历

> ※ 该题目和二叉树的层次遍历类似，均采用了相同的框架，只是在向队列中添加节点时有所不同（这里直接调用了`addAll()`函数，关于该函数，后面有时间可以仔细看一下）。

```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> res = new LinkedList<List<Integer>>();
        if (root == null) {
            return res;
        }
        Queue<Node> queue = new LinkedList<Node>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int count = queue.size();
            List<Integer> list = new LinkedList<Integer>();
            for (int i = 0; i < count; i++) {
                Node node = queue.poll();
                list.add(node.val);
                // Way 1:
                // List<Node> children = node.children;
                // for (int i = 0; i < children.size(); i++) {
                //     queue.offer(children.get(i));
                // }
                // Way 2:
                // for (Node child : node.children) {
                //     queue.offer(child);
                // }
                // Way 3:
                queue.addAll(node.children);
            }
            res.add(list);
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 513. 找树左下角的值

> ※ 最先想到的方法是：从上往下，从左往右依次遍历二叉树每一层的节点，在遍历每一层节点之前，先用一个临时变量保存当前层的第一个节点的值，直至遍历到最后一层。然而，这样的方法需要额外定义两个临时变量，并且程序结构较为复杂，是否存在更为简单明了的方法？答案是肯定的（见代码）。

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while(!queue.isEmpty()) {
            root = queue.poll();
            if (root.right != null) {
                queue.offer(root.right);
            }
            if (root.left != null) {
                queue.offer(root.left);
            }
        }
        return root.val;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 654. 最大二叉树

> ※ 该题目比较简单，利用递归即可进行求解。

```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return buildTree(nums, 0, nums.length - 1);
    }
    private TreeNode buildTree(int[] nums, int l, int r) {
        if (l > r) {
            return null;
        }
        int index = getMaxIndex(nums, l, r);
        TreeNode node = new TreeNode(nums[index]);
        node.left = buildTree(nums, l, index - 1);
        node.right = buildTree(nums, index + 1, r);
        return node;
    }
    private int getMaxIndex(int[] nums, int l, int r) {
        int max_value = Integer.MIN_VALUE, max_index = -1;
        for (int i = l; i <= r; i++) {
            if (max_value < nums[i]) {
                max_value = nums[i];
                max_index = i;
            }
        }
        return max_index;
    }
}
```

> ※ 时间复杂度：$O(N^2)$；空间复杂度：$O(N)$。

#### 701. 二叉搜索树中的插入操作

> ※ 遍历当前节点，假设当前节点的值小于待插入的值，若当前节点的右孩子不为空，则继续遍历当前节点的右孩子；若当前节点的右孩子为空，则直接为当前节点新建一个右孩子，并将右孩子的值赋值为待插入的值。反之亦然。

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }
        if (root.val < val) {
            if (root.right == null) {
                root.right = new TreeNode(val);
            } else {
                insertIntoBST(root.right, val);
            }
        } else {
            if (root.left == null) {
                root.left = new TreeNode(val);
            } else {
                insertIntoBST(root.left, val);
            }
        }
        return root;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

#### 1302. 层数最深叶子节点的和

> ※ 对二叉树进行层次遍历，每遍历一层便将该层所有节点的和保存至`res`中，待程序运行结束时，`res`中保存的便为二叉树最深叶子节点的和。

```java
class Solution {
    public int deepestLeavesSum(TreeNode root) {
        int res = 0;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        if (root == null) {
            return res;
        }
        queue.offer(root);
        while (!queue.isEmpty()) {
            res = 0;
            int count = queue.size();
            while (count > 0) {
                count--;
                TreeNode node = queue.poll();
                res += node.val;
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return res;
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

-----

### 困难

-----

#### 968. 监控二叉树

> ※ 对于某个节点来说，可能存在以下`3`种状态：`0`，当前节点需要被监控；`1`：当前节点已经被监控；`2`：当前节点需要放置摄像头。弄清楚这`3`种状态便能够轻松解决该题目。需要注意的是，不要忘记对根节点的状态进行判断，若根节点处于状态`0`，则需要将摄像头的个数加一。

```java
class Solution {
    private int res = 0;
    public int minCameraCover(TreeNode root) {
        if (root == null) {
            return 0;
        } else {
            if (DFS(root) == 0) { // 不要忘记根节点！！！
                res++;
            }
            return res;
        }
    }
    // 0: 需要被监控；
    // 1: 已经被监控；
    // 2: 需要放置摄像头；
    private int DFS(TreeNode node) {
        if (node == null) {
            // 若 node 为空，则 node 已经被监控；
            return 1;
        } else {
            int left = DFS(node.left), right = DFS(node.right);
            if (left == 1 && right == 1) {
                // 若 left 和 right 均为 1，则 node 需要被监控；
                return 0;
            } else if (left == 0 || right == 0) {
                // 若 left 或 right 为 0，则 node 需要放置摄像头；
                res++;
                return 2;
            } else {
                // 其余情况包含 12 21 22，此时 node 已经被监控；
                return 1;
            }
        }
    }
}
```

> ※ 时间复杂度：$O(N)$；空间复杂度：$O(N)$。

-----