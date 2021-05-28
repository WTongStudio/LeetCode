# [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

## 解题思路

层次遍历每一层节点，并交换每一个节点的左右子节点。

## 复杂度分析

**时间复杂度：O(NlogK)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func mirrorTree(root *TreeNode) *TreeNode { // 基于层次遍历思想
	if root == nil {                        // 特判
		return nil
	}
	queue := list.New()
	queue.PushBack(root)
	for queue.Len() > 0 {
		size := queue.Len()
		for i := 0; i < size; i++ { // 遍历每一层
			node := queue.Remove(queue.Front()).(*TreeNode)
			node.Left, node.Right = node.Right, node.Left // 交换每一个节点的左右子节点
			if node.Left != nil {
				queue.PushBack(node.Left)
			}
			if node.Right != nil {
				queue.PushBack(node.Right)
			}
		}
	}
	return root
}
```