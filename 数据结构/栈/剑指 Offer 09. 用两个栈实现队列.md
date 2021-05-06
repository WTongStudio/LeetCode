# [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

## 解题思路

维护两个栈，第一个栈支持插入操作，第二个栈支持删除操作。

根据栈先进后出的特性，每次往第一个栈里插入元素后，第一个栈的底部元素是最后插入的元素，第一个栈的顶部元素是下一个待删除的元素。为了维护队列先进先出的特性，**引入第二个栈**，用第二个栈维护待删除的元素，在执行删除操作的时候我们首先看下第二个栈是否为空。**如果为空，将第一个栈里的元素一个个弹出插入到第二个栈里**，这样第二个栈里元素的顺序就是待删除的元素的顺序，要执行删除操作的时候我们直接弹出第二个栈的元素返回即可。

## 复杂度分析

**时间复杂度：O(1)**，对于插入和删除操作，时间复杂度均为 O(1)。插入不多说，对于删除操作，虽然看起来是 O(n) 的时间复杂度，但是仔细考虑下每个元素只会**「至多被插入和弹出 stack2 一次」，因此均摊下来每个元素被删除的时间复杂度仍为 O(1)**。

**空间复杂度：O(N)** 

## 代码实现

```golang
type CQueue struct {
	stack1 list.List // 用链表模拟栈1
	stack2 list.List // 用链表模拟栈2
}

func Constructor() CQueue { // 工厂方法
	return CQueue{}
}

func (this *CQueue) AppendTail(value int) { // 在队列尾部插入数据
	// 添加数据前检查栈2是否为空
	if this.stack2.Len() == 0 { // 若栈2为空，则将栈1中的数据逐一导入栈2
		for this.stack1.Len() > 0 {
			// 链表模拟出入栈
			this.stack2.PushBack(this.stack1.Remove(this.stack1.Back()))
		}
	}
	this.stack1.PushBack(value)
}

func (this *CQueue) DeleteHead() int { // 在队列头部删除数据
	// 删除数据前检查栈2是否为空
	if this.stack2.Len() == 0 { // 若栈2为空，则将栈1中的数据逐一导入栈2
		for this.stack1.Len() > 0 {
			// 链表模拟出入栈
			this.stack2.PushBack(this.stack1.Remove(this.stack1.Back()))
		}
	}
	if this.stack2.Len() == 0 {
		return -1
	}
	head := this.stack2.Remove(this.stack2.Back())
	return head.(int)
}
```