---
title: 01 树
date: 2020-09-24 11:46:30
categories:
- Leetcode
tags:
- 树
---

### 简单

-----

#### 501. 二叉搜索树中的众数

> ※ 想了一段时间后实在想不出来，就按照官方题解实现了一下。狗屎，时间复杂度和空间复杂度完全比不上利用递归的时间复杂度和空间复杂度。好在借此机会了解了一下真·$O(1)$空间复杂度的`Morris`遍历方法，如果有题目规定不能使用额外的空间，包括由递归产生的隐式调用栈的开销，那么该方法就派的上用场了。另外，在此分享一篇关于介绍`Morris`遍历方法的不错的<a href="https://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html">博客</a>，后面有时间可以仔细看一看。
>
> ```java
> class Solution {
>      private int base, count, maxCount;
>      private List<Integer> res = new ArrayList<Integer>();
>      public int[] findMode(TreeNode root) {
>            TreeNode node = root;
>            while (node != null) {
>                if (node.left == null) {
>                    // 若当前节点的左孩子为空，则输出当前节点，并将当前节点更新为它的右孩子。
>                    counter(node.val);
>                    node = node.right;
>                } else {
>                    TreeNode preNode = node.left;
>                    // 若当前节点的左孩子不为空，则在其左子树中找到其前驱节点。
>                    while (preNode.right != null && preNode.right != node) {
>                        preNode = preNode.right;
>                    }
>                    if (preNode.right == null) {
>                        // 若前驱节点的右孩子为空，则将其右孩子设置为当前节点，
>                        // 并将当前节点更新为当前节点的左孩子。
>                        preNode.right = node;
>                        node = node.left;
>                    } else {
>                        // 若前驱节点的右孩子为当前节点，则将其右孩子重新设为空（恢复树的形状），
>                        // 输出当前节点，并将当前节点更新为当前节点的右孩子。
>                        preNode.right = null;
>                        counter(node.val);
>                        node = node.right;
>                    }
>                }
>            }
>            int[] mode = new int[res.size()];
>            for (int i = 0; i < res.size(); i++) {
>                mode[i] = res.get(i);
>            }
>            return mode;
>      }
>      private void counter(int val) {
>            if (val == base) {
>                count++;
>            } else {
>                count = 1;
>                base = val;
>            }
>            if (count == maxCount) {
>                res.add(val);
>            } else if (count > maxCount) {
>                maxCount = count;
>                res.clear();
>                res.add(val);
>            }
>      }
> }
> ```
>
> ※ 时间复杂度：$O(n)$；空间复杂度：$O(1)$。

> **TODO**：该题目还可以利用递归解决，并且运行效果更好，后面有时间可以试一下。

#### 617. 合并二叉树

> ※ 两个二叉树中对应的节点存在以下几种情况：一个节点为空，另一个节点不为空，此时合并后的节点应该为不为空的那个节点；两个节点均不为空，此时合并后的节点为其中任意一个节点即可，但需要对返回的节点的值进行修改；两个节点均为空，此时合并后的节点为其中任意一个节点即可，反正都为空。
>
> ```java
> class Solution {
>      public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
>            if (t1 != null && t2 != null) {
>                t1.val += t2.val;
>                t1.left = mergeTrees(t1.left, t2.left);
>                t1.right = mergeTrees(t1.right, t2.right);
>                return t1;
>            } else if (t1 == null && t2 == null) {
>                return null;
>            } else if (t1 != null && t2 == null) {
>                return t1;
>            } else {
>                return t2;
>            }
>      }
> }
> ```
>
> ※ 时间复杂度：$O(m, n)$，其中`m`、`n`分别表示两个二叉树的节点数，只有当两个二叉树中对应的节点均不为空时，才会对两个节点进行显性合并操作，因此被访问到的节点数不会超过较小的二叉树的节点数；空间复杂度：$O(m, n)$，空间复杂度取决于递归调用的层数，而递归调用的层数不会超过较小的二叉树的节点数。

-----

### 困难

-----

#### 968. 监控二叉树

> ※ 对于某个节点来说，可能存在以下`3`种状态：`0`，当前节点需要被监控；`1`：当前节点已经被监控；`2`：当前节点需要放置摄像头。弄清楚这`3`种状态便能够轻松解决该题目。需要注意的是，不要忘记对根节点的状态进行判断，若根节点处于状态`0`，则需要将摄像头的个数加一。
>
> ```java
> class Solution {
>     private int res = 0;
>     public int minCameraCover(TreeNode root) {
>         if (root == null) {
>             return 0;
>         } else {
>             if (DFS(root) == 0) { // 不要忘记根节点！！！
>                 res++;
>             }
>             return res;
>         }
>     }
>     // 0: 需要被监控；
>     // 1: 已经被监控；
>     // 2: 需要放置摄像头；
>     private int DFS(TreeNode node) {
>         if (node == null) {
>             // 若 node 为空，则 node 已经被监控；
>             return 1;
>         } else {
>             int left = DFS(node.left), right = DFS(node.right);
>             if (left == 1 && right == 1) {
>                 // 若 left 和 right 均为 1，则 node 需要被监控；
>                 return 0;
>             } else if (left == 0 || right == 0) {
>                 // 若 left 或 right 为 0，则 node 需要放置摄像头；
>                 res++;
>                 return 2;
>             } else {
>                 // 其余情况包含 12 21 22，此时 node 已经被监控；
>                 return 1;
>             }
>         }
>     }
> }
> ```
> 
> ※ 时间复杂度：$O(n)$；空间复杂度：$O(n)$。

-----