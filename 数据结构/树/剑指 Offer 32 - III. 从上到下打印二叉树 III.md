# [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

## 解题思路

**层次遍历+双端遍历**，层次遍历，对奇偶层方向输出即可。

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(N)** 

## 代码实现

```golang
func levelOrder(root *TreeNode) [][]int {
	var res [][]int
	if root == nil { // 特判
		return res
	}
	queue := list.New()
	queue.PushBack(root)
	for queue.Len() > 0 {
		size := queue.Len()
		level := make([]int, 0)
		for i := 0; i < size; i++ {
			if len(res)%2 == 0 { // 偶数层，正向操作
				node := queue.Remove(queue.Front()).(*TreeNode)
				level = append(level, node.Val)
				if node.Left != nil {
					queue.PushBack(node.Left)
				}
				if node.Right != nil {
					queue.PushBack(node.Right)
				}
			} else { // 奇数层，方向操作，注意插入节点时也要从前插入
				node := queue.Remove(queue.Back()).(*TreeNode)
				level = append(level, node.Val)
				if node.Right != nil { // 插入时左右也需反过来
					queue.PushFront(node.Right)
				}
				if node.Left != nil { // 插入时左右也需反过来
					queue.PushFront(node.Left)
				}
			}
		}
		res = append(res, level)
	}
	return res
}
```