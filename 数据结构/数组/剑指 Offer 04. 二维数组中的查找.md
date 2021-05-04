# [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

## 解题思路

**解题关键是查找的起始位置**，若从左上角开始找，两个方向上都是递增，无法确定搜索方向，因此可以从右上角开始查找，向左是递减，向下是递增。

## 复杂度分析

**时间复杂度：O(N+M)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func findNumberIn2DArray(matrix [][]int, target int) bool {
	if len(matrix) == 0 { // 特判
		return false
	}
	rows := len(matrix)
	cols := len(matrix[0])
	for i, j := 0, cols-1; j >= 0 && i < rows; { // 关键：从右上角开始查找
		if matrix[i][j] > target { // 若大于目标值，则向左找
			j--
		} else if matrix[i][j] < target { // 若小于目标值，则向下找
			i++
		} else {
			return true
		}

	}
	return false
}
```