# [剑指 Offer 10. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

## 解题思路

斐波那契数列问题，可以通过递推迭代方式求解，该有很多变种，例如：[剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)。或是用 2x1 的小矩形横着或者竖着去覆盖更大的矩形，问用 8 个 2x1 的小矩形无重叠地覆盖一个 2x8 的大矩形，总共有多少种方法？解法都是一样的，**但需要注意处理类型溢出问题**！

![9E766F70-1454-4B47-BC43-D77654934DDD](/Users/wtongstudio/Desktop/LeetCode/数据结构/数组/images/9E766F70-1454-4B47-BC43-D77654934DDD.png)

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(1)** 

## 代码实现

```golang
func fib(n int) int {
	if n < 2 {
		return n
	}
	a, b := 0, 1
	for i := 2; i <= n; i++ {
		a, b = b, (a+b)%1000000007 // 注意处理类型溢出，在此处而非返回时
	}
	return b
}
```