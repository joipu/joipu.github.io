---
layout: single
title:  "Linked List (this is for test)"
date:   2019-05-28 21:27:56 -0700
categories: jekyll test
---
## Linked List (this is for test)

1. Linked List vs Array
+ 都属于linear array
+ Linked list's 优点
	- 链表是一个动态的数据结构，其容量可以随意增大和减小
	- 链表不需要在初始化时，提前申请用于存储元素的内存
	- 链表是一个将多个局部节点连接起来的数据结构（类似的还有树和图）
	- 插入删除不需要移动其他元素，操作起来非常简单 (Complexity = O(1))
+ Linked list's 缺点
	- 链表比数组要占用更多的内存，因为链表除了需要用于值存储之外，还有一些指针也需要占用空间
	- 读取链表的节点时，必须要从表头或者表尾开始一个一个按顺序读 (Complexity = O(n))
	- 从链表中查找元素是比较费时的，因为链表中的节点不是连续存储的，所以访问单个节点的效率比较低

2. Elements (4):
+ data
+ reference to the next node (Node next & Node previous)
+ head
+ tail (optional)

3. Operations:
+ Update Node next and Node previous
+ Update Node head and Node tail
+ Check for the NULL pointer

4. Notes:
+ Two pointer technique
+ Recursive problems: If having trouble solving a linked list problem, explore if a recursive approach will work. (Complexity = O(n) where n = the depth of the recursive call)
+ In interview, you must check whether it is a singly linked list or a doubly linked list

5. Implementation of a basic singly linked list (must-know, recite):
```
	class Node {
		// initialization: 1) a pointer to next 2) value of current node
		Node next = null;
		int data;

		// constructor with a value
		public Node(int d) {
			data = d;
		}

		// operation: append node to the tail
		void appendToTail(int d) {
			Node n = this;
			Node end = new Node(d);
			while (n.next != null) {
				n = n.next;
			}
			n.next = end;
		}
	}
```

6. Implementation of deleting a node from linked list (must-know, recite):
```
	public Node removeNode(Node head, int d) {
		Node n = head;
		
		// remove head
		if (n.data == d) {
			return head.next;
		}

		// remove node inside
		while (n.next != null) {
			if (n.next.data == d) {
				n.next = n.next.next;
				return head;
			}
			n = n.next;
		}

		return head;
	}
```