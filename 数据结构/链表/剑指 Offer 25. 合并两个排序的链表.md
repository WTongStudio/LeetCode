# [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

## 解题思路

使用哨兵节点优化可以简化链表操作，注意处理结尾。

## 复杂度分析

**时间复杂度：O(M+N)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
	dummyHead := &ListNode{} // 哨兵节点优化，简化操作
	prev := dummyHead
	node1, node2 := l1, l2
	for node1 != nil && node2 != nil {
		if node1.Val <= node2.Val {
			prev.Next = node1
			prev = node1
			node1 = node1.Next
		} else {
			prev.Next = node2
			prev = node2
			node2 = node2.Next
		}
	}
	// 注意处理结尾
	if node1 != nil {
		prev.Next = node1
	}
	if node2 != nil {
		prev.Next = node2
	}
	return dummyHead.Next
}
```