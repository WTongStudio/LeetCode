# [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

## 解题思路

本题在 LeetCode 中只要求在 int 范围内，相对简单，解法略。面试时通常是考验大数处理问题，当 n 很大时无法用 int、long 等任意类型直接表示，**需要用字符串/数组模拟大数**。

基于分治算法的思想，先固定高位，向低位递归，当个位已被固定时，添加数字的字符串。例如当 n=2 时（数字范围 1−99 ），固定十位为 9 ，按顺序依次开启递归，固定个位 9 ，终止递归并添加数字字符串。

![Picture1.png](images/83f4b5930ddc1d42b05c724ea2950ee7f00427b11150c86b45bd88405f8c7c87-Picture1.png)

注意，需要处理前导 0 问题：

**字符串左边界定义**： 声明变量 start 规定字符串的左边界，以保证添加的数字字符串 num[start:] 中无高位多余的 
0 。例如当 n=2 时， 1−9 时 start=1 ， 10−99 时 start=0 。

**左边界 start 变化规律**： 观察可知，当输出数字的所有位都是 9 时，则下个数字需要向更高位进 1 ，此时左边界 start 需要减 1 （即高位多余的 0 减少一个）。例如当 n=3 （数字范围 1−999 ）时，左边界 start 需要减 1 的情况有： "009" 进位至 "010" ， "099" 进位至 "100" 。设数字各位中 9 的数量为 nine ，所有位都为 9 的判断条件可用以下公式表示：n-start=nine。

**统计 nine 的方法**： 固定第 x 位时，当 i=9 则执行 nine=nine+1 ，**并在回溯前恢复 nine=nine−1** 。

## 复杂度分析

**时间复杂度：O(10^N)**

**空间复杂度：O(10^N)**，若需记录全部结果。 

## 代码实现

```golang
import "fmt"

var loop = []byte{'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'}

func printNumbers(n int) {
	if n <= 0 { // 特判
		return
	}
	bigNum := make([]byte, n)
	nine := 0      // 记录 9 的个数
	start := n - 1 // 输出的起始位置，处理前导 0 问题
	for i := 0; i < 10; i++ {
		bigNum[0] = loop[i]
		dfs(0, n, bigNum, &nine, &start) // 深度遍历
	}
}

func dfs(index, n int, bigNum []byte, nine, start *int) {
	if index == n-1 { // 终止条件
		fmt.Println(string(bigNum[*start:]))
		if n-*start == *nine { // 满足进位条件：009->010、099->100等
			*start-- // 即高位多余的 0 减少一个
		}
		return
	}
	for i := 0; i < 10; i++ {
		if i == 9 { // 统计 9 的个数
			*nine++
		}
		bigNum[index+1] = loop[i]
		dfs(index+1, n, bigNum, nine, start)
	}
	*nine-- // 注意回溯时恢复 nine 的值
}
```