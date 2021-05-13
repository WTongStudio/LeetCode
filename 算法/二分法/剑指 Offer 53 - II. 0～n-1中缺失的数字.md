# [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

## 解题思路

二分法变种，查找到第一个不满足条件的位置，**特别注意，若未找到，说明缺失的是最后一个值**。

## 复杂度分析

**时间复杂度：O(logN)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func missingNumber(nums []int) int {
	low, high := 0, len(nums)-1
	for low <= high {
		mid := low + (high-low)>>1
		if nums[mid] != mid {
			// 二分法变种：查找到第一个不满足条件的位置
			if mid-1 >= 0 && nums[mid-1] != mid-1 {
				high = mid - 1
			} else {
				return mid
			}
		} else {
			low = mid + 1
		}
	}
	// 关键：若未找到不满足条件的位置，说明缺失的是最后一个
	return len(nums)
}
```

## 代码实现（简化逻辑）

```golang
func missingNumber(nums []int) int {
	low, high := 0, len(nums)-1
	for low <= high {
		mid := low + (high-low)>>1
		if nums[mid] == mid {
			low = mid + 1 // low 总是指向不满足条件的范围
		} else {
			high = mid - 1 // 不满足条件时，缩小范围
		}
	}
	return low
}
```

