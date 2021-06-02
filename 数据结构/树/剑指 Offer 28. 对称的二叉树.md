# [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

## 方法一：递归

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
	if p1.Val != p2.Val { // 两个镜像二叉树的根节点必然相等 
		return false
	}
	return isSymmetricRecursion(p1.Left, p2.Right) && isSymmetricRecursion(p1.Right, p2.Left)
}
```

## 方法二：递推

## 解题思路

首先我们引入一个队列，这是把递归程序改写成迭代程序的常用方法。初始化时我们把根节点入队两次。每次提取两个节点并比较它们的值（队列中每两个连续的结点应该是相等的，而且它们的子树互为镜像），**然后将两个结点的左右子结点按相反的顺序插入队列中**。当队列为空时，或者我们检测到树不对称（即从队列中取出两个不相等的连续结点）时，该算法结束。

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(N)**

## 代码实现

```go
func isSymmetric(root *TreeNode) bool {
	queue := make([]*TreeNode, 0)
	queue = append(queue, root)
	queue = append(queue, root)
	for len(queue) > 0 {
		p1, p2 := queue[0], queue[1]
		queue = queue[2:]
		if p1 == nil && p2 == nil {
			continue
		}
		if p1 == nil || p2 == nil {
			return false
		}
		if p1.Val != p2.Val {
			return false
		}
		queue = append(queue, p1.Left)
		queue = append(queue, p2.Right)
		queue = append(queue, p1.Right)
		queue = append(queue, p2.Left)
	}
	return true
}
```

