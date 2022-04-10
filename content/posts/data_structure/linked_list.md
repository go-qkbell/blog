---
title: "[DataStructure] Singly Linked List"  
date: 2022-04-09
categories: ["Go", "DataStructure", "EN"]  
tags: ["Go", "DataStructure", "EN"]

summary: What is linked list

weight: 1
math: true
draft: false
---

## What is Singly Linked List?

![DataStructure](/../images/linkedlist.png)

- Linked list is a linear collection of data.
- Linked List is **NOT** stored in physically contiguous memory.
- Linked List's length can be increased/decreased in runtime.
- Instead, Linked list contains node that points to the next.

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
Linked list has head and tail, which is node's address.<br>
Node consist of its own value and address to the next node.<br>

### Add Node to Linked List
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
![DataStructure](/../images/singlylist1.png)
![DataStructure](/../images/singlylist2.png)
![DataStructure](/../images/singlylist3.png)

Adding at the back of the linked list costs **O(1)**.<br>
Because all you have to do is make linked list's current tail node to point to the new node.<br>
Then change linked list's tail to the new node.<br>
<br>
![DataStructure](/../images/singlylist1.png)
![DataStructure](/../images/singlylist4.png)
Adding at the front also costs **O(1)**.<br>
Because all you have to do is make new node that points to linked list's current head node.<br>
Then change linked list's head to the new node.<br>
<br>
However, adding at the given index is **O(N)**, because you have to traverse until the index.<br>
<br>
Remember how array and slice can access index O(1)?<br>
Because array and slice's  memory is physically contiguous.<br>
But since linked list's memory is **not** physically contiguous, you have to traverse all the way by following nodes' next addresses.

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
![DataStructure](/../images/singlylist5.png)
Deleting front node of the linked list costs **O(1)**.<br>
Just set your head node to current head node's next node.<br>

![DataStructure](/../images/singlylist6.png)
However deleting node at the back costs **O(N)**.<br>
Because you don't know current tail node's previous node. (which is going to be a new tail node)<br>
You have to traverse all the way to tail node's previous node.<br>
Then delete tail node and set tail node's previous node to tail. <br>

Deleting specific node also costs **O(N)**. <br>
You don't know node's previous node, so you have to traverse to find previous node.

### Singly Linked List Time Complexity

- Insert at head: **O(1)**
- Insert at tail: **O(1)**
- Insert at index: **O(N)**
- Remove at head: **O(1)**
- Remove at tail: **O(N)**
- Remove at index: **O(N)**
- Search: **O(N)**

There is improved linked list called **Doubly Linked List**.<br>
We will talk about that in the next post.


## References

- https://www.geeksforgeeks.org/data-structures/linked-list/
- https://www.programiz.com/dsa/linked-list