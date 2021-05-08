# [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

## 解题思路

递归思想判断，每个节点是否对称。

## 复杂度分析

**时间复杂度：O(N)**，每次递归判断一个节点是否对称。

**空间复杂度：O(N)**，递归栈空间。 

## 代码实现

```golang
func isSymmetric(root *TreeNode) bool {
	return isSymmetricRecursion(root, root)
}

func isSymmetricRecursion(p1 *TreeNode, p2 *TreeNode) bool {
	if p1 == nil && p2 == nil {
		return true
	}
	if p1 == nil || p2 == nil {
		return false
	}
	if p1.Val != p2.Val {
		return false
	}
	return isSymmetricRecursion(p1.Left, p2.Right) && isSymmetricRecursion(p1.Right, p2.Left)
}
```