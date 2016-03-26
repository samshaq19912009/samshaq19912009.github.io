---
layout: post
title: Reverse Linked List
date: 2016-03-26
categories: blog
tags: [Leetcode,Cpp]
description: Reverse
---


Leetcode上一道easy的题目，涉及了最基本的链表的翻转。有些基础知识需要牢固，然后牢记清楚每次翻转的目的和具体步骤，应该就不会出错了。


## Iterative Solution ##

### Solution 1 ###


```cpp
class Solution {
public:
	LinkNode *reverse(LinkNode *head) {
		if(!head) return head;
		ListNode *prev = nullptr;
		ListNode *cur = head;
		while(cur != nullptr) {
			ListNode *temp = cur->next;
			//not need to implement this line
			//prev->next = temp;
			cur->next = prev;
			prev = cur;
			cur = temp;
		}
		return prev;
	}
	
};

```




### Solution 2 ###

```cpp
class Solution {
public:
	LinkNode *reverse(LinkNode *head) {
		if(!head) return head;
		LinkNode dummy(-1);
		dummy.next = head;
		
		ListNode *cur = head;
		while(cur->next) {
			ListNode* temp = cur->next;
			cur->next = temp->next; // skip this node
			temp->next = dummy.next; // make this node the first node
			dummy.next = temp;//mark the begin
		}
		return dummy.next;
	}
};

```



## Recursive Solution ##

```cpp
class Solution {
public:
	LinkNode *reverse(LinkNode *head) {
		if (head == nullptr || head->next == nullptr)
			return head;
		ListNode* temp = head->next;
		ListNode* newhead = reverse(temp);
		head->next = nullptr;
		temp->next = head; 

		return newhead;
	}
};

```




