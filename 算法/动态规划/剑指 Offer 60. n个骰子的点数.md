# [剑指 Offer 60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)

## 解题思路

![04B81346-CF21-4955-BAE9-B4A512674F29](images/04B81346-CF21-4955-BAE9-B4A512674F29.png)

![24041CE7-5FF9-4AE5-9F6C-B79F92AFDA79](images/24041CE7-5FF9-4AE5-9F6C-B79F92AFDA79.png)

![16467083-ECF7-45BE-BF79-7CB4CA3DA7D5](images/16467083-ECF7-45BE-BF79-7CB4CA3DA7D5.png)

![70AEE317-C4B0-4105-AAD7-4E7C0EBCDF80](images/70AEE317-C4B0-4105-AAD7-4E7C0EBCDF80.png)

![B5940FEC-0388-4354-A671-8AF076509E41](images/B5940FEC-0388-4354-A671-8AF076509E41.png)

## 复杂度分析

**时间复杂度：O(N^2)**

**空间复杂度：O(N)** 

## 代码实现

```golang
func dicesProbability(n int) []float64 {
	dp := make([]float64, 6)
	for i := range dp { // 初始化
		dp[i] = 1 / 6.0
	}
	for i := 2; i <= n; i++ { // 从2个筛子开始遍历所有筛子
		tmp := make([]float64, 5*i+1)
		for j := 0; j < len(dp); j++ {
			for k := 0; k < 6; k++ {
				tmp[j+k] += dp[j] / 6.0
			}
		}
		dp = tmp
	}
	return dp
}
```