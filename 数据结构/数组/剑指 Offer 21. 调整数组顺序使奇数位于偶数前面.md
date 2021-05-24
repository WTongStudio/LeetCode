# [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

## 方法一：首尾双指针

## 解题思路

两个指针分别指向数组首尾，当前指针指向偶数，后指针指向奇数时，交互两个数组元素，否则缩小范围继续查找。**可以使用位运算来快速高效的判断奇偶**。

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func exchange(nums []int) []int {
	low, high := 0, len(nums)-1
	for low < high {
		if nums[low]&1 != 0 { // 位运算判断奇偶数
			low++
			continue
		}
		if nums[high]&1 != 1 {
			high--
			continue
		}
		nums[low], nums[high] = nums[high], nums[low]
	}
	return nums
}
```

## 方法二：快慢双指针

## 解题思路

两个指针从 0 开始遍历，一快一慢，当前数组元素为奇数时，说明位置正确（奇数在前面），快慢指针均后移。当前数组元素为偶数时，慢指针不动，快指针继续后移寻找后面的奇数元素，若找到则交换快慢指针指向的数组元素，若未找到则表示当前已经是最终解。

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func exchange(nums []int) []int {
	slow, fast := 0, 0
	for fast < len(nums) {
		if nums[fast]&1 == 1 { // 若为奇数，则交换数组元素，slow后移
			nums[slow], nums[fast] = nums[fast], nums[slow]
			slow++
		}
		fast++
	}
	return nums
}
```

