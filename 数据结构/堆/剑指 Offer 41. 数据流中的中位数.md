# [剑指 Offer 41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

## 解题思路

![8009FF9B-4BED-481C-83EE-FC157D22522B](images/8009FF9B-4BED-481C-83EE-FC157D22522B.png)

![3D78A7EA-EA15-4B4D-8F7B-B4482F81DE13](images/3D78A7EA-EA15-4B4D-8F7B-B4482F81DE13.png)

## 复杂度分析

**时间复杂度：O(logN)**，堆的插入和弹出操作使用 O(log⁡N) 时间。

**空间复杂度：O(N)**，其中 N 为数据流中的元素数量，小顶堆 A 和大顶堆 B 最多同时保存 N 个元素。 

## 代码实现

```golang
type Heap []int

func (m *Heap) Swap(i, j int) {
	(*m)[i], (*m)[j] = (*m)[j], (*m)[i]
}

func (m *Heap) Len() int {
	return len(*m)
}

func (m *Heap) Pop() (v interface{}) {
	*m, v = (*m)[:len(*m)-1], (*m)[len(*m)-1]
	return
}

func (m *Heap) Push(v interface{}) {
	*m = append(*m, v.(int))
}

type minHeap struct {
	Heap
}

func (m *minHeap) Less(i, j int) bool {
	return m.Heap[i] < m.Heap[j]
}

type maxHeap struct {
	Heap
}

func (m *maxHeap) Less(i, j int) bool {
	return m.Heap[i] > m.Heap[j]
}

type MedianFinder struct {
	LeftMax  *maxHeap // 前半部分数据构建大顶堆
	RightMin *minHeap // 后半部分数据构建小顶堆
}

/** initialize your data structure here. */
func Constructor() MedianFinder { // 工厂方法
	m := new(MedianFinder)
	m.RightMin = new(minHeap)
	m.LeftMax = new(maxHeap)
	heap.Init(m.LeftMax)  // 利用 golang 的 heap 接口实现堆
	heap.Init(m.RightMin) // 利用 golang 的 heap 接口实现堆
	return *m
}

func (this *MedianFinder) AddNum(num int) {
	// 压入数据时有两种情况；
	// 1）原有数据为偶数个，则有 LeftMax.len() == RightMin.len()
	//   此时将数据放入 LeftMax
	//   高明之处：需先将数据放在 RightMin，再从 RightMin 中 pop 出一个元素，将其放入 LeftMax
	//   省去了判断 num 与 RightMin最小值 及 LeftMax最大值 的比较，通过比较判断 num 应该放在哪个 heap
	//   然后再根据左右 heap 的长度，对左右 heap 进行调整
	// 2) 原有数据为奇数个，则有 LeftMax.len() == RightMin.len() + 1
	//   此时将数据放入 RightMin
	//   类似上一步，需先将数据放在 LeftMax，再从 LeftMax 中 pop 出一个元素，将其放入 RightMin 中
	if this.LeftMax.Len() == this.RightMin.Len() {
		heap.Push(this.RightMin, num)
		heap.Push(this.LeftMax, heap.Pop(this.RightMin))
	} else {
		heap.Push(this.LeftMax, num)
		heap.Push(this.RightMin, heap.Pop(this.LeftMax))
	}
}

func (this *MedianFinder) FindMedian() float64 {
	if (this.RightMin.Len()+this.LeftMax.Len())%2 == 0 {
		// 取堆顶元素
		return float64(this.LeftMax.Heap[0]+this.RightMin.Heap[0]) / 2
	} else {
		// 取堆顶元素
		return float64(this.LeftMax.Heap[0])
	}
}
```