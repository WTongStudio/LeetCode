# [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

## 解题思路

二分法的变种，首先查找到第一个等于目标值的位置，然后遍历计数即可。

## 复杂度分析

**时间复杂度：O(logN)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func search(nums []int, target int) int {
	low, high := 0, len(nums)-1
	for low <= high {
		mid := low + (high-low)>>1
		if nums[mid] == target {
			// 二分法变种：查找第一个等于目标值的位置
			if mid-1 >= 0 && nums[mid-1] == target {
				high = mid - 1
			} else {
				cnt := 1
				for i := mid + 1; i < len(nums); i++ {
					if nums[i] != nums[mid] {
						break
					}
					cnt++
				}
				return cnt
			}
		} else if nums[mid] < target {
			low = mid + 1
		} else {
			high = mid - 1
		}
	}
	return 0
}
```