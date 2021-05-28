# [剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

## 解题思路（自底向上递归）

**自顶向下**递归，因此对于同一个节点，函数 height 会被重复调用，导致时间复杂度较高。如果使用**自底向上**的做法，则对于每个节点，函数 height 只会被调用一次。自底向上递归的做法类似于后序遍历，对于当前遍历到的节点，先递归地判断其左右子树是否平衡，再判断以当前节点为根的子树是否平衡。如果一棵子树是平衡的，则返回其高度（高度一定是非负整数），否则返回 −1。**如果存在一棵子树不平衡，则整个二叉树一定不平衡**。

![C0D1AC59-645D-41C2-80FF-88E8670F1514](images/C0D1AC59-645D-41C2-80FF-88E8670F1514.png)

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(N)** 

## 代码实现

```golang
func isBalanced(root *TreeNode) bool {
	return height(root) != -1
}

func height(root *TreeNode) int { // 若非平衡二叉树返回-1，否则返回树的高度
	if root == nil {
		return 0
	}
	left := height(root.Left)
	if left == -1 { // 左子树非平衡
		return -1
	}
	right := height(root.Right)
	if right == -1 { // 右子树非平衡
		return -1
	}
	if abs(left-right) > 1 { // 不满足平衡二叉树条件
		return -1
	}
	return max(left, right) + 1
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func abs(a int) int {
	if a < 0 {
		return -1 * a
	}
	return a
}
```
