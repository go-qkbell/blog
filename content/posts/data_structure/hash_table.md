---
title: "[DataStructure] Hash Table"  
date: 2022-04-14
categories: ["Go", "DataStructure", "EN"]  
tags: ["Go", "DataStructure", "EN"]

summary: What is Hash Table

weight: 1
math: true
draft: false
---

## What is Hash Table?
![DataStructure](/../images/hashtable.png)

- Key, Value data structure
- Very fast insert, delete, and search **O(1)**
- Using hash function to choose bucket index

Internally hash table uses array(bucket). <br>
Remember, array costs **O(N)** to search.<br>
But hash table costs **O(1)**. How does that work?<br>
<br>
It's because hash table uses hash function to determine which index is to be used.

## What is Hash Function?
- Hash Function transforms data into fixed length
- Hash Function always gives same output for same input

When you put key into hash function, it outputs hash code, and you can use it for index.<br>
Instead of iterating the whole array, you just have to look up the index(hash code).<br>

Hash Function is a key role in hash table.<br>
Good hash function must be fast, efficient, and outputs unique hash code.<br>

This looks like the best data structure.<br>
Is hash table perfect?<br>
Unfortunately it's not. Because eventually, there is going to be a hash collision.<br>

## What is Hash Collision?
![DataStructure](/../images/hash_collision.png)
Hash collision is when more than two keys share the same hash code.<br>
This happens because hash function gets us a small number for a key which is a big integer or string or etc.<br>
Even though hash algorithms have been created with the intent of being collision resistant, it's still not perfect.<br>
This will be a conflict when there are lots of hash collisions and many data gets stored in particular bucket.

## Collision Resolution Method
### Open Addressing
![DataStructure](/../images/open_address.png)
Open addressing is one of the collision resolution method.<br>
Open address uses empty space in the bucket.<br>
<br>
Linear Probing
- linearly move N times until you find empty slot and store there.<br>

Quadratic Probing
- linearly move N^2 times until you find empty slot and store there.<br>
<br>

Problem of open addressing
- Size of bucket<br>
-- Size of bucket must be same or greater than keys to store all the keys.

- Data clustering<br>
-- Many data form groups in bucket, and it will start to take time to find a free slot or to search for an index.<br>
-- Need to re-hash hash table when elements are deleted.

### Separate Chaining
![DataStructure](/../images/hash_chaining.png)
Separate chaining uses extra memory to store value when collision happens.<br>
It creates linked list at bucket and keep adding value at tail when collision occurs.<br>
This is simple to implement and bucket array never fills up, but if the chain becomes long, search time will be **O(N)** because it's a linked list.
<br>
<br>
I will use separate chaining in below hash table implement
### Implement Hash Table

```go
package main

import "fmt"

const ArraySize = 7

type HashTable struct {
	array [ArraySize]*bucket
}

type bucket struct {
	head *bucketNode
}

type bucketNode struct {
	key  string
	next *bucketNode
}

func (h *HashTable) Insert(key string) {
	index := hash(key)
	h.array[index].insert(key)
}

func (h *HashTable) Search(key string) bool {
	index := hash(key)
	return h.array[index].search(key)
}

func (h *HashTable) Delete(key string) {
	index := hash(key)
	h.array[index].delete(key)
}

func (b *bucket) insert(k string) {
	if !b.search(k) {
		newNode := &bucketNode{key: k}
		newNode.next = b.head
		b.head = newNode
	} else {
		fmt.Println("Already Exists")
	}

}

func (b *bucket) search(k string) bool {
	currentNode := b.head
	for currentNode != nil {
		if currentNode.key == k {
			return true
		}
		currentNode = currentNode.next
	}

	return false
}

func (b *bucket) delete(k string) {
	if b.head.key == k {
		b.head = b.head.next
	} else {
		prevNode := b.head
		for prevNode.next != nil {
			if prevNode.next.key == k {
				prevNode.next = prevNode.next.next
			}
			prevNode = prevNode.next
		}
	}
}

func hash(key string) int {
	sum := 0
	for _, v := range key {
		sum += int(v)
	}

	return sum % ArraySize
}
```

### Hash Table Time Complexity

- Insert: **O(1)**
- Remove: **O(1)**
- Search: **O(1)**

**BUT** hash function and collision really matters in time complexity when using hash table.<br>
So it is very important to implement efficient hash function.