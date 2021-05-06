# [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

## 解题思路

本问题是典型的矩阵搜索问题，可使用 **深度优先搜索（DFS）+ 剪枝** 解决。

- **深度优先搜索**： DFS 通过递归，先朝一个方向搜到底，再回溯至上个节点，沿另一个方向搜索，以此类推。
- **剪枝**： 在搜索中，遇到这条路不可能和目标字符串匹配成功的情况（例如：此矩阵元素和目标字符不同、此元素已被访问），则应立即返回，称之为**可行性剪枝** 。

![Picture0.png](images/1604944042-glmqJO-Picture0.png)

## 复杂度分析

**时间复杂度：O(3^K*(MN))**，最差情况下，需要遍历矩阵中长度为 K 字符串的所有方案，搜索中每个字符有上、下、左、右四个方向可选，舍弃回头（上个字符）方向，剩下 3 种选择，因此方案数的时间复杂度为 O(3^K)，矩阵中共有 MN 个起点，时间复杂度为 O(MN) 。 

**空间复杂度：O(K)** ，搜索过程中的递归深度不超过 K ，因此系统因函数调用累计使用的栈空间占用 O(K) （因为函数返回后，系统调用的栈空间会释放）。最坏情况下 K=MN ，递归深度为 MN ，此时系统栈使用 O(MN) 的额外空间。

## 代码实现

```golang
func exist(board [][]byte, word string) bool {
	n, m := len(board), len(board[0])
	for i := 0; i < n; i++ {
		for j := 0; j < m; j++ {
			if dfs(board, word, n, m, i, j, 0) {
				return true
			}
		}
	}
	return false
}

func dfs(board [][]byte, word string, n, m, i, j, k int) bool {
	if i < 0 || i >= n || j < 0 || j >= m { // 边界判断
		return false
	}
	if board[i][j] != word[k] {
		return false
	}
	if k == len(word)-1 { // 终止条件
		return true
	}
	board[i][j] = 0 // 标记为ascii的0值，防止重复访问
	// 深度搜索上下左右四个方向
	res := dfs(board, word, n, m, i+1, j, k+1) ||
		dfs(board, word, n, m, i-1, j, k+1) ||
		dfs(board, word, n, m, i, j+1, k+1) ||
		dfs(board, word, n, m, i, j-1, k+1)
	board[i][j] = word[k] // 恢复原始数据
	return res
}
```