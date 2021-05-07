# [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

## 解题思路

链表反转基本操作，注意细节。

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func reverseList(head *ListNode) *ListNode {
	var prev *ListNode // 创建空节点作为前驱节点，无需创建哨兵节点
	curr := head
	for curr != nil {
		next := curr.Next // 先记录当前节点的下一节点，防覆盖丢失
		curr.Next = prev
		prev = curr
		curr = next
	}
	return prev
}
```