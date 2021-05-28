# [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

## 解题思路

本问题是典型的二叉树方案搜索问题，使用回溯法解决，其包含 **先序遍历 + 路径记录** 两部分。

**先序遍历**： 按照 “根、左、右” 的顺序，遍历树的所有节点。
**路径记录**： 在先序遍历中，记录从根节点到当前节点的路径。当路径 **根节点到叶节点形成的路径** 且 **各节点值的和等于目标值 sum 时**，将此路径加入结果列表。

![2CC178D7-4CC4-46BE-A66F-83072FF83E4B](images/2CC178D7-4CC4-46BE-A66F-83072FF83E4B.png)

![B7E54F71-64F7-4A89-9216-52D1845971CB](images/B7E54F71-64F7-4A89-9216-52D1845971CB.png)

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(N)** 

## 代码实现

```golang
func pathSum(root *TreeNode, target int) [][]int {
	var res [][]int
	var path []int
	var dfs func(root *TreeNode)
	dfs = func(root *TreeNode) {
		if root == nil {
			return
		}
		target = target - root.Val
		path = append(path, root.Val)
		if target == 0 && root.Left == nil && root.Right == nil { // 叶子节点
			// path 切片的底层数组可能会被修改
			// 所以需要先 append 到空切片，在 append 到 ret 中
			// 防止 path 底层数据修改后，导致加入到 ret 的切片也发生变化
			res = append(res, append([]int{}, path...))
			target = target + root.Val
			path = path[:len(path)-1]
			return
		}
		dfs(root.Left)
		dfs(root.Right)
		target = target + root.Val
		path = path[:len(path)-1]
	}
	dfs(root)
	return res
}
```

## 相关题目

[面试题 04.12. 求和路径](https://github.com/WTongStudio/LeetCode/blob/master/数据结构/树/面试题%2004.12.%20求和路径.md)