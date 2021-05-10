# [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

## 解题思路

构建小顶堆。

## 复杂度分析

**时间复杂度：O(NlogK)**，每次插入数据进堆的时间复杂度为 O(logK)，一共扫描插入 N 次。

**空间复杂度：O(1)** ，原地建堆处理。

## 代码实现

```golang
func heapify(tree []int, n, i int) {
	// 堆化操作调整结构（构造小顶堆），调整顺序是从上向下
	// 递归实现
	if i > n {
		return
	}
	c1 := 2*i + 1 // 左子节点
	c2 := 2*i + 2 // 右子节点
	max := i
	if c1 < n && tree[c1] > tree[max] {
		max = c1
	}
	if c2 < n && tree[c2] > tree[max] {
		max = c2
	}
	if max != i {
		tree[max], tree[i] = tree[i], tree[max]
		heapify(tree, n, max)
	}
}

func getLeastNumbers(arr []int, k int) []int {
	n := len(arr)
	// 1、构建大顶堆，根据前 k 个元素构建
	for i := k/2 - 1; i >= 0; i-- {
		// 从第一个非叶子结点从下至上，从右至左调整结构
		// n/2-1 指向最下层最右边的非叶子节点
		heapify(arr, k, i)
	}
	// 2、动态添加后续元素，从第 k 个元素开始添加
	for i := k; i < n; i++ {
		if arr[i] < arr[0] { // 若添加元素小于堆顶元素
			// 替换对应元素
			arr[i], arr[0] = arr[0], arr[i]
			// 重新堆化调整
			heapify(arr, k, 0)
		}
	}
	return arr[:k]
}
```

## 相关题目

[215. 数组中的第K个最大元素](https://github.com/WTongStudio/LeetCode/blob/master/数据结构/堆/215.%20数组中的第K个最大元素.md)