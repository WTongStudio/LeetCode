# [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

## 解题思路

本题与 [剑指 Offer 59 - II. 队列的最大值](https://github.com/WTongStudio/LeetCode/blob/master/数据结构/队列/剑指%20Offer%2059%20-%20II.%20队列的最大值.md) 类似方法类似，窗口相当于一个队列，右移相当于队列的出队及入队，可以通过一个双端队列来维护队列中的最大值。

## 复杂度分析

**时间复杂度：O(NlogK)**

**空间复杂度：O(1)** 

## 代码实现

```golang
type MaxQueue struct {
	queue *list.List // 链表模拟队列
	deque *list.List // 链表模拟双向队列
}

func Constructor() MaxQueue {
	return MaxQueue{list.New(), list.New()}
}

func (this *MaxQueue) Max_value() int {
	if this.deque.Len() == 0 {
		return -1
	}
	return this.deque.Front().Value.(int)
}

func (this *MaxQueue) Push_back(value int) {
	this.queue.PushBack(value)
	for this.deque.Len() > 0 && this.deque.Back().Value.(int) < value {
		this.deque.Remove(this.deque.Back())
	}
	this.deque.PushBack(value)
}

func (this *MaxQueue) Pop_front() int {
	if this.queue.Len() == 0 {
		return -1
	}
	front := this.queue.Remove(this.queue.Front()).(int)
	if this.deque.Front().Value.(int) == front { // 出队元素与双向队列中的队首元素相等
		this.deque.Remove(this.deque.Front())
	}
	return front
}

func maxSlidingWindow(nums []int, k int) []int {
	var res []int
	if len(nums) == 0 {
		return res
	}
	q := Constructor()
	for i := 0; i < k; i++ { // 初始化
		q.Push_back(nums[i])
	}
	res = append(res, q.Max_value())
	for i := k; i < len(nums); i++ { // 滑动窗口
		// 左边元素出队
		q.Pop_front()
		// 右边元素入队
		q.Push_back(nums[i])
		res = append(res, q.Max_value())
	}
	return res
}
```

## 相关题目

[剑指 Offer 59 - II. 队列的最大值](https://github.com/WTongStudio/LeetCode/blob/master/数据结构/队列/剑指%20Offer%2059%20-%20II.%20队列的最大值.md)