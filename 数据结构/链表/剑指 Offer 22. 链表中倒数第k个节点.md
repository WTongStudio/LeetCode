# [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

## 解题思路

使用快慢指针，找的倒数第 k 个节点。

## 复杂度分析

**时间复杂度：O(NlogK)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func getKthFromEnd(head *ListNode, k int) *ListNode {
	slow, fast := head, head
	for i := 0; i < k; i++ {
		fast = fast.Next
	}
	for fast != nil {
		slow = slow.Next
		fast = fast.Next
	}
	return slow
}
```