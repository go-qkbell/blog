---
title: "[DataStructure] Heap"  
date: 2022-04-20
categories: ["Go", "DataStructure", "EN"]  
tags: ["Go", "DataStructure", "EN"]

summary: What is Heap

weight: 1
math: true
draft: false
---

## What is Heap?
![DataStructure](/../images/heap.png)

- Heap can be expressed as complete tree
- Root node has the highest priority<br> (we will talk about max heap in this post, min heap is just opposite of max heap)
- Every parent node is bigger than its children node
- Heap looks like a tree, but it's actually an array(slice)
- Getting the highest priority key (root node) is **O(1)**
- Heapify process costs **O(h)** which is height of the heap

Heap is especially good at implementing priority queue.<br>
In normal queues, we pop values in order because queue is FIFO(first-in, first-out) data structure.<br>
But in priority queue, we take out the highest priority value first.<br>

## Heap in array representation
![DataStructure](/../images/heap_array.png)

As I said, heap is actually an array.<br>
Heap can be represented in array, because it is easy to calculate child node's index.<br>
<br>
**H[k]'s left child node := H[2 * k + 1]**<br>
**H[k]'s right child node := H[2 * k + 2]**<br>
<br>
**H[k]'s parent node := H[(k-1) / 2]**

## Heap Insertion
![DataStructure](/../images/heap_insert.png)
![DataStructure](/../images/heap_insert2.png)
![DataStructure](/../images/heap_insert3.png)
Whenever we insert node in a heap, we will insert at bottom right.<br>
Which will be the last index in array form.<br>
After that, we need to re-arrange the heap.<br>
If new node is bigger than parent node, then we will swap two nodes and repeat it until node finds its place.<br>
This process is called **heapify**.<br>
Heapify process is also needed when deleting node.

## Heap Deletion
![DataStructure](/../images/heap_delete0.png)
![DataStructure](/../images/heap_delete.png)
To take out the biggest number in a heap, just pop the root node, which is index [0].<br>
After that, re-arrange is needed because we need to fill in the empty root.<br>
So, move the last node to the root.<br>
Then start heapify process from the top.<br>
Compare root node with child nodes. Then swap with bigger node, which is 50 in this example.<br>
Repeat this process until node finds its place.<br>

## Code
```go
package heap

type MaxHeap struct {
	array []int
}

func (h *MaxHeap) Insert(key int) {
	h.array = append(h.array, key)
	h.maxHeapifyUp(len(h.array) - 1)
}

func (h *MaxHeap) maxHeapifyUp(index int) {
	for h.array[parent(index)] < h.array[index] {
		h.swap(parent(index), index)
		index = parent(index)
	}
}

func (h *MaxHeap) Extract() int {
	extracted := h.array[0]

	if len(h.array) == 0 {
		return -1
	}

	h.array[0] = h.array[len(h.array)-1]
	h.array = h.array[:len(h.array)-1]

	h.maxHeapifyDown(0)

	return extracted
}

func (h *MaxHeap) maxHeapifyDown(index int) {
	lastIndex := len(h.array) - 1
	l, r := left(index), right(index)
	childToCompare := 0

	for l <= lastIndex {
		if l == lastIndex {
			childToCompare = l
		} else if h.array[l] > h.array[r] {
			childToCompare = l
		} else {
			childToCompare = r
		}

		if h.array[index] < h.array[childToCompare] {
			h.swap(index, childToCompare)
			index = childToCompare
			l, r = left(index), right(index)
		} else {
			return
		}
	}
}

func (h *MaxHeap) swap(i1, i2 int) {
	h.array[i1], h.array[i2] = h.array[i2], h.array[i1]
}

func parent(i int) int {
	return (i - 1) / 2
}

func left(i int) int {
	return i*2 + 1
}

func right(i int) int {
	return i*2 + 2
}
```

### Heap Time Complexity
- Heapify: **O(h)** = **O(log n)**