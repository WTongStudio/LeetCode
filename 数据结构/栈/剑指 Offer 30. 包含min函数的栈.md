# [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

## 解题思路

**本题难点是将 min() 函数复杂度降为 O(1) ，可以通过辅助栈实现**。

**数据栈 A** ：用于存储所有元素，保证入栈 push() 函数、出栈 pop() 函数、获取栈顶 top() 函数的正常逻辑。
**辅助栈 B** ：用于存储栈 A 中所有**非严格降序**的元素，则栈 A 中的最小元素始终对应栈 B 的栈顶元素，即 min() 函数只需返回栈 B 的栈顶元素即可。

**注意 pop() 时，需要判断弹出元素是否与辅助栈 B 的栈顶元素相等，若相等则需同时弹出**。

## 复杂度分析

**时间复杂度：O(1)**

**空间复杂度：O(N)** 

## 代码实现

```golang
import (
	"container/list"
)

type MinStack struct {
	stackA *list.List // 数据栈 A
	stackB *list.List // 辅助栈 B，用于存储栈 A 中所有非严格降序的元素
}

/** initialize your data structure here. */
func Constructor() MinStack { // 工厂方法
	return MinStack{stackA: list.New(), stackB: list.New()}
}

func (this *MinStack) Push(x int) {
	this.stackA.PushBack(x)
	if this.stackB.Len() > 0 {
		if this.stackB.Back().Value.(int) >= x {
			this.stackB.PushBack(x)
		}
	} else {
		this.stackB.PushBack(x)
	}
}

func (this *MinStack) Pop() {
	// 弹出时判断是否与辅助栈 B 的栈顶元素相等，若相等则需同时弹出
	if this.Top() == this.stackB.Back().Value.(int) {
		this.stackB.Remove(this.stackB.Back())
	}
	this.stackA.Remove(this.stackA.Back())
}

func (this *MinStack) Top() int {
	return this.stackA.Back().Value.(int)
}

func (this *MinStack) Min() int {
	return this.stackB.Back().Value.(int)
}

/**
 * Your MinStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * obj.Pop();
 * param_3 := obj.Top();
 * param_4 := obj.Min();
 */
```