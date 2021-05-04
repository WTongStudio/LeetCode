# [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

## 解题思路

需要注意到**数组中的数字都在 0~n-1 范围内**，如果没有重复数字，那么正常排序后，数字 i 应该在下标为 i 的位置，所以思路是重头扫描数组，遇到下标为 i 的数字如果不是 i 的话（假设为m），那么就拿当前值与下标 m 的数字交换，即将当前值放入正确的位置（对号入座）。在交换过程中，如果当前值与其正确位置中的值相等，则表示有重复，终止返回。

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func findRepeatNumber(nums []int) int {
	for i := 0; i < len(nums); i++ { // 遍历所有位置
		for i != nums[i] { // 若当前位置的数字不是对号入座，则交换至正确位置
			if nums[i] == nums[nums[i]] { // 若正确位置已经有对号入座的数字，表示有重复
				return nums[i] // 返回任意一个重复数字
			}
			// 将当前数字交换至正确位置
			nums[i], nums[nums[i]] = nums[nums[i]], nums[i]
		}
	}
	return -1
}
```

## 拓展延伸

若题目改为需要返回所有重复的数字（不是任意一个），则需要微调代码，进行丢弃策略。

## 代码实现

```golang
func findRepeatNumber(nums []int) []int {
	var res []int
	dict := make(map[int]bool)
	for i := 0; i < len(nums); i++ { // 遍历所有位置
		for i != nums[i] { // 若当前位置的数字不是对号入座，则交换至正确位置
			if nums[i] == nums[nums[i]] { // 若正确位置已经有对号入座的数字，表示有重复
				if !dict[nums[i]] { // 结果集去重
					res = append(res, nums[i]) // 记录所有重复数字
					dict[nums[i]] = true
				}
				break
			}
			// 将当前数字交换至正确位置
			nums[i], nums[nums[i]] = nums[nums[i]], nums[i]
		}
	}
	return res
}
```

