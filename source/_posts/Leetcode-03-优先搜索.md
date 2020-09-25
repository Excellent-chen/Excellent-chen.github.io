---
title: 03 优先搜索
date: 2020-09-24 14:53:11
categories:
- Leetcode
tags:
- 优先搜索
---

### 中等

-----

#### 130. 被围绕的区域

> ※ 该题目可以抽象成图的深度优先搜索，比较简单。

```java
class Solution {
    private boolean[][] visited;
    public void solve(char[][] board) {
        if (board.length == 0) {
            return;
        }
        visited = new boolean[board.length][board[0].length];
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (i != 0 && i != board.length - 1 && j != 0 && j != board[0].length - 1) {
                    continue;
                }
                if (board[i][j] == 'O' && visited[i][j] == false) {
                    visited[i][j] = true;
                    DFS(board, i, j);
                }
            }
        }
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == 'O' && visited[i][j] == false) {
                    board[i][j] = 'X';
                }
            }
        }
    }
    private void DFS(char[][] board, int x, int y) {
        int[][] directions = {{0, -1}, {0, 1}, {-1, 0}, {1, 0}};
        for (int[] direction : directions) {
            int dx = x + direction[0], dy = y + direction[1];
            if (0 <= dx && dx < board.length && 0 <= dy && dy < board[0].length) {
                if (board[dx][dy] == 'O' && visited[dx][dy] == false) {
                    visited[dx][dy] = true;
                    DFS(board, dx, dy);
                }
            }
        }
    }
}
```

> ※ 时间复杂度：$O(mn)$；空间复杂度：$O(mn)$。

> **TODO**：在解决该题目时借助了一个与`board`同样大小的`boolean`数组，若题目规定不能声明新的数组，此时你知道该怎么办嘛？

#### 200. 岛屿数量

> ※ 该题目可以抽象成图的深度优先搜索，比较简单。

```java
class Solution {
    private boolean[][] visited;
    public int numIslands(char[][] grid) {
        int res = 0;
        if (grid.length == 0) {
            return 0;
        }
        visited = new boolean[grid.length][grid[0].length];
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1' && visited[i][j] == false) {
                    res++;
                    DFS(grid, i, j);
                }
            }
        }
        return res;
    }

    private void DFS(char[][] grid, int x, int y) {
        visited[x][y] = true;
        int[][] directions = {{0, -1}, {0, 1}, {-1, 0}, {1, 0}};
        for (int[] direction : directions) {
            int dx = x + direction[0], dy = y + direction[1];
            if (0 <= dx && dx < grid.length && 0 <= dy && dy < grid[0].length) {
                if (grid[dx][dy] == '1' && visited[dx][dy] == false) {
                    visited[dx][dy] = true; // 利用 visited 防止重复访问
                    DFS(grid, dx, dy);
                }
            }
        }
    }
}
```

> ※ 时间复杂度：$O(mn)$；空间复杂度：$O(mn)$。

> **TODO**：该题目同样可以利用广度优先搜索和并查集解决，并且这两种方法的复杂度在某种程度上均优于深度优先搜索，后面有时间可以试一下。

-----