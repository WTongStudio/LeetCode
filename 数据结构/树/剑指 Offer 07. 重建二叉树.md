# [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

## 解题思路

对于任意一颗树而言，前序遍历的形式总是：

```
[ 根节点, [左子树的前序遍历结果], [右子树的前序遍历结果] ]
```

对于任意一颗树而言，中序遍历的形式总是：

```
[ [左子树的中序遍历结果], 根节点, [右子树的中序遍历结果] ]
```

**本题目的关键是所有节点值不重复！**因此，可以在中序遍历中定位到根节点，然后就可以分别知道左子树和右子树中的**节点数目**。由于同一颗子树的**前序遍历和中序遍历的长度是相同的**，因此可以分别定位到左右子树的前序、中序遍历序列，重复上述步骤，即可递归构建二叉树。

**优化细节**：在中序遍历中对根节点进行定位时，一种简单的方法是直接扫描整个中序遍历的结果并找出根节点，但这样做的时间复杂度较高。**由于所有节点不重复**，可以考虑使用哈希表辅助快速地定位根节点。对于哈希映射中的每个键值对，键表示一个元素（节点的值），值表示其在中序遍历中的出现位置。在构造二叉树的过程之前，可以对中序遍历的列表进行一遍扫描，构建出哈希映射。此后构造二叉树的过程中，只需要 O(1) 的时间对根节点进行定位。

## 复杂度分析

**时间复杂度：O(N)**

**空间复杂度：O(N)** 

## 代码实现（未优化）

```golang
func buildTree(preorder []int, inorder []int) *TreeNode {
	//if len(preorder) == 0 || len(inorder) == 0 {
	// 特判，由于前序遍历与中序遍历的节点数目是一样的，所以此处只需判断前序遍历长度
	if len(preorder) == 0 {
		return nil
	}
	root := &TreeNode{Val: preorder[0]} // 根节点
	n := len(preorder)
	for i := 0; i < n; i++ { // 遍历中序遍历序列，查找根节点（preorder[0]）位置
		if inorder[i] == preorder[0] {
			root.Left = buildTree(preorder[1:i+1], inorder[:i])   // 左子树的前序+中序遍历序列
			root.Right = buildTree(preorder[i+1:], inorder[i+1:]) // 右子树的前序+中序遍历序列
			break
		}
	}
	return root
}
```

## 代码实现（哈希优化）

```golang
func buildHash(inorder []int) map[int]int { // 构建中序序列的哈希表映射
	index := make(map[int]int)
	for i := 0; i < len(inorder); i++ {
		index[inorder[i]] = i
	}
	return index
}

func buildTree(preorder []int, inorder []int) *TreeNode {
	index := buildHash(inorder) // 构建中序序列的哈希表映射
	return buildTreeByIndex(preorder, inorder, index, 0, len(preorder)-1, 0, len(inorder)-1)
}

func buildTreeByIndex(preorder, inorder []int, index map[int]int, pLeft, pRight, iLeft, iRight int) *TreeNode {
	if pLeft > pRight { // 终止条件
		return nil
	}
	root := &TreeNode{Val: preorder[pLeft]} // 根节点
	rootIndex := index[preorder[pLeft]]     // 快速定位根节点在中序遍历序列中的位置
	// 得到左子树中的节点数目
	leftTreeNum := rootIndex - iLeft
	// 递归地构造左子树，并连接到根节点
	// 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
	root.Left = buildTreeByIndex(preorder, inorder, index, pLeft+1, pLeft+leftTreeNum, iLeft, rootIndex-1)
	// 递归地构造右子树，并连接到根节点
	// 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
	root.Right = buildTreeByIndex(preorder, inorder, index, pLeft+leftTreeNum+1, pRight, rootIndex+1, iRight)
	return root
}
```

