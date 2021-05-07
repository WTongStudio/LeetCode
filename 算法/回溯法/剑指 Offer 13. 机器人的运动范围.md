# [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

## 方法一：深度优先搜索

## 解题思路

经典的深度优先搜索算法，注意需要创建二维数组标记访问位置。

## 复杂度分析

**时间复杂度：O(MN)**

**空间复杂度：O(MN)** 

## 代码实现

```golang
var res int

func movingCount(m int, n int, k int) int {
	visited := make([][]bool, m) // 标记访问过的位置，防止重复访问
	for i := range visited {
		visited[i] = make([]bool, n)
	}
	// 深度优先搜索
	dfs(m, n, k, 0, 0, visited)
	return res
}

func dfs(m, n, k, i, j int, visited [][]bool) {
	if i >= m || j >= n || visited[i][j] { // 终止条件
		return
	}
	if count(i)+count(j) > k { // 障碍位置
		return
	}
	res++
	visited[i][j] = true // 标记
  // 只需搜索向右和向下两个方向即可，无需回头搜索
	dfs(m, n, k, i+1, j, visited) // 向下搜索
	dfs(m, n, k, i, j+1, visited) // 向右搜索
}

func count(n int) int { // 计算各位之和
	cnt := 0
	for n != 0 {
		cnt += n % 10
		n = n / 10
	}
	return cnt
}
```

## 方法二：广度优先搜索

## 解题思路

使用辅助队列实现广度优先搜索算法，思路类似二叉树的层次遍历。

## 复杂度分析

**时间复杂度：O(MN)**

**空间复杂度：O(MN)** 

## 代码实现

```golang
func movingCount(m int, n int, k int) int {
	if k == 0 {
		return 1
	}
	visited := make([][]bool, m) // 标记访问过的位置，防止重复访问
	for i := range visited {
		visited[i] = make([]bool, n)
	}
	visited[0][0] = true
	// 使用辅助队列实现广度优先搜索，思路类似二叉树的层次遍历
	queue := list.New() // 链表模拟队列
	queue.PushBack([2]int{0, 0})
	dx := []int{0, 1} // 向右方向数组
	dy := []int{1, 0} // 向下方向数组
	res := 1
	for queue.Len() > 0 {
		pair := queue.Remove(queue.Front()).([2]int)
		x, y := pair[0], pair[1]
		for i := 0; i < 2; i++ {
			tx := dx[i] + x
			ty := dy[i] + y
			if tx < 0 || tx >= m || ty < 0 || ty >= n || visited[tx][ty] {
				continue
			}
			if count(tx)+count(ty) > k {
				continue
			}
			queue.PushBack([2]int{tx, ty})
			visited[tx][ty] = true
			res++
		}
	}
	return res
}
```

