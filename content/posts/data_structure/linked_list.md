---
title: "[DataStructure] Linked List"  
date: 2022-04-09
categories: ["Go", "DataStructure", "EN"]  
tags: ["Go", "DataStructure", "EN"]

summary: What is linked list

weight: 1
math: true
draft: false
---

## What is Linked List?

![DataStructure](/../images/linkedlist.png)

- Linked list is a linear collection of data.
- Linked List is **NOT** stored in physically contiguous memory.
- Linked List's length can be increased/decreased in run time.
- Instead, Linked list has node that points to the next.

### Implement Linked List

```go
package main

import (
	"fmt"
)

type SinglyLinkedList struct {
	head   *SinglyNode
	tail   *SinglyNode
	length int
}

type SinglyNode struct {
	next  *SinglyNode
	value int
}

func NewSinglyList() *SinglyLinkedList {
	return &SinglyLinkedList{
		head:   nil,
		tail:   nil,
		length: 0,
	}
}

func NewSinglyNode(v int) *SinglyNode {
	return &SinglyNode{
		next:  nil,
		value: v,
	}
}

func main() {
	newList := NewSinglyList()
	fmt.Println(newList)    // &{<nil> <nil> 0}
}
```
Linked list has head and tail, which is node.<br>
Node consist of its own value and address of next node.<br>

### Add Node to Linked List

![DataStructure](/../images/add_front_list_1.png)
![DataStructure](/../images/add_front_list_2.png)

```go
func (s *SinglyLinkedList) AddFront(n *SinglyNode) {
	if s.head == nil {
		s.head = n
		s.tail = n
	} else {
		n.next = s.head
		s.head = n
	}

	s.length++
}

func (s *SinglyLinkedList) AddBack(n *SinglyNode) {
	if s.tail == nil {
		s.head = n
		s.tail = n
	} else {
		s.tail.next = n
		s.tail = n
	}

	s.length++
}

func (s *SinglyLinkedList) AddAt(n *SinglyNode, idx int) {
	if idx == 0 {
		s.AddFront(n)
		return
	} else if idx > s.length {
		s.AddBack(n)
		return
	} else {
		node := s.head
		for i := 0; i < idx-1; i++ {
			node = node.next
		}
		n.next = node.next
		node.next = n
	}

	s.length++
}
```
Adding in the front or back of the linked list costs O(1).<br>
Because all you have to do is make node and let linked list's head or tail point to it, and change links between nodes.<br>
<br>
However, adding in the given index is O(N), because you have to traverse until the index.<br>
Remember how array and slice can access index in O(1)?<br>
Because their memories are physically contiguous.<br>
But since linked list memory is not physically contiguous, you have to follow along by nodes' next address.

### Delete Node in Linked List
```go
func (s *SinglyLinkedList) RemoveFront() {
	if s.head == nil {
		return
	}
	if s.head == s.tail {
		s.head = nil
		s.tail = nil
	} else {
		s.head = s.head.next
	}

	s.length--
}

func (s *SinglyLinkedList) RemoveBack() {
	if s.tail == nil {
		return
	}
	if s.head == s.tail {
		s.head = nil
		s.tail = nil
	} else {
		for n := s.head; n != nil; {
			if n.next == s.tail {
				s.tail = n
				n.next = nil
			}
			n = n.next
		}
	}

	s.length--
}

func (s *SinglyLinkedList) RemoveNode(n *SinglyNode) {
	if s.head == n {
		s.head = s.head.next
		s.length--
		return
	}

	for pn := s.head; pn != nil; {
		if pn.next == n {
			pn.next = n.next

			if n == s.tail {
				s.tail = pn
			}

			break
		}

		pn = pn.next
	}

	s.length--
}
```
Deleting front node of the linked list costs O(1).<br>
However deleting node at the back costs O(N).<br>
Because since you don't know tail's previous node, you have to traverse all the way to tail node's previous node.<br>
Deleting specific node costs O(N). <br>
You don't know node's previous node, so you have to traverse to find previous node.
