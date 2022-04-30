---
title: "[DataStructure] Binary Search Tree"  
date: 2022-04-27
categories: ["Go", "DataStructure", "EN"]  
tags: ["Go", "DataStructure", "EN"]

summary: What is BST

weight: 1
math: true
draft: false
---

## What is Binary Search Tree?
![DataStructure](/../images/binarysearchtree.png)

- Sorted tree data structure
- Every parent node has two or fewer children node
- Every left child node is smaller than parent node
- Every right child node is bigger than parent node
- Insert and search cost **O(h)** which is height of the heap (Best: O(log n), Worst: O(N))

## Binary Search Tree Insertion
![DataStructure](/../images/bst_insert.png)

When new node is inserted, compare it with root node.<br>
If the new node is bigger than the root node, then place right.<br>
If the new node is smaller than the root node, then place left.<br>
If there is another node at that place, then do the same process until the node finds free spot. (recursion)

## Binary Search Tree Search
![DataStructure](/../images/bst_search.png)

Search process is same as insertion.<br>
If the node is bigger than the root node, then go right.<br>
If the node is smaller than the root node, then go left.<br>
Recursively compare node until you find.<br>

## Problem with Binary Search Tree
![DataStructure](/../images/BinTreeWorstCase.png)

However in real life, binary search trees are not always perfect.<br>
It might overweight to one side.<br>
If this happens, time complexity is going to be **O(h)** == **O(N)**.<br>
So we need to re-balance the tree. (AVL tree, Red-black tree etc.)

## Code
```go
package BinarySearchTree

type Node struct {
	key   int
	left  *Node
	right *Node
}

func (n *Node) Insert(key int) {
	if n.key < key {
		if n.right == nil {
			n.right = &Node{key: key}
		} else {
			n.right.Insert(key)
		}
	} else if n.key > key {
		if n.left == nil {
			n.left = &Node{key: key}
		} else {
			n.left.Insert(key)
		}
	}
}

func (n *Node) Search(key int) bool {
	if n == nil {
		return false
	}

	if n.key < key {
		return n.right.Search(key)
	} else if n.key > key {
		return n.left.Search(key)
	}

	return true
}
```

### Binary Search Tree Time Complexity (well-balanced case)
- Insert: **O(h)** = **O(log n)**
- Search: **O(h)** = **O(log n)**