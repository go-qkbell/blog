---
title: "[DataStructure] Doubly Linked List"  
date: 2022-04-13
categories: ["Go", "DataStructure", "EN"]  
tags: ["Go", "DataStructure", "EN"]

summary: What is doubly linked list

weight: 1
math: true
draft: false
---

## What is Doubly Linked List?

![DataStructure](/../images/doublylist.png)

- Doubly Linked List is just like Singly Linked List, but with **previous node**.

### Implement Doubly Linked List

```go
package main

type DoublyLinkedList struct {
	head   *DoublyNode
	tail   *DoublyNode
	length int
}

type DoublyNode struct {
	next  *DoublyNode
	prev  *DoublyNode
	value int
}

func NewDoublyList() *DoublyLinkedList {
	return &DoublyLinkedList{
		head:   nil,
		tail:   nil,
		length: 0,
	}
}

func NewDoublyNode(v int) *DoublyNode {
	return &DoublyNode{
		next:  nil,
		prev:  nil,
		value: v,
	}
}
```

Doubly Linked List has extra node that points to previous node's address.


### Add Node to Doubly Linked List
```go
func (s *DoublyLinkedList) AddFront(n *DoublyNode) {
	if s.head == nil {
		s.head = n
		s.tail = n
	} else {
		n.next = s.head
		s.head.prev = n
		s.head = n
	}

	s.length++
}

func (s *DoublyLinkedList) AddBack(n *DoublyNode) {
	if s.tail == nil {
		s.head = n
		s.tail = n
	} else {
		n.prev = s.tail
		s.tail.next = n
		s.tail = n
	}

	s.length++
}

func (s *DoublyLinkedList) AddAt(n *DoublyNode, idx int) {
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
		n.prev = node
		node.next.prev = n
		node.next = n

	}

	s.length++
}
```

### Delete Node in Singly Linked List
```go
func (s *DoublyLinkedList) RemoveFront() {
	if s.head == nil {
		return
	}
	if s.head == s.tail {
		s.head = nil
		s.tail = nil
	} else {
		s.head = s.head.next
		s.head.prev = nil
	}

	s.length--
}

func (s *DoublyLinkedList) RemoveBack() {
	if s.tail == nil {
		return
	}
	if s.head == s.tail {
		s.head = nil
		s.tail = nil
	} else {
		s.tail = s.tail.prev
		s.tail.next = nil
	}

	s.length--
}

func (s *DoublyLinkedList) RemoveNode(n *DoublyNode) {
	if s.head == n {
		s.RemoveFront()
		return
	}
	if n == s.tail {
		s.RemoveBack()
		return
	}

	pn := n.prev

	pn.next = pn.next.next
	pn.next.prev = pn

	s.length--
}
```
Singly Linked List costs O(N) when removing tail node.<br>
Because Singly Linked List does not know where tail node's previous node is.<br>
However Doubly Linked List has information about previous node. So it just costs **O(1)**. <br>
You just have to set Doubly Linked List's tail to current tail node's previous node.<br>

### Doubly Linked List Time Complexity

- Insert at head: **O(1)**
- Insert at tail: **O(1)**
- Insert at index: **O(N)**
- Remove at head: **O(1)**
- Remove at tail: **O(1)**
- Remove at index: **O(N)**
- Access Index: **O(N)**
- Search: **O(N)**