---
layout: post
title: Remove duplicate
date: 2016-04-06
categories: blog
tags: [Leetcode,Notes]
description: Remove duplicate
---

``` cpp
class Solution {
 public:
  ListNode *deleteDuplicate(ListNode *head) {
    if (!head || !head->next) return head;
    ListNode *p = head->next;
    if (head->val == p->val) {
      while(p && head->val == p->val) {
	ListNode *temp = p;
	p = p->next;
	delete temp;
      }
      delete head;
      return deleteDuplicate(p);
    } else {
      head->next = deleteDuplicate(head->next);
      return head;
    }
  }
};
//iterative solution

class Solution {
 public:
  ListNode *deleteDuplicate(ListNode *head) {
    if (head == nullptr) return head;
    ListNode dummy(head->val+1);
    dummy.next = head;
    ListNode *prev = dummy, *cur = head;
    while(cur != nullptr) {
      bool duplicated = false;
      while (cur->next != nullptr && cur->val == cur->next->val) {
	duplicated = true;
	ListNode *temp = cur;
	cur = cur->next;
	delete temp;
      }
      if (duplicated) {
	ListNode *temp = cur;
	cur = cur->next;
	delete temp;
	continue;
      }
      prev->next = cur;
      prev = prev->next;
      cur = cur->next;
    }
    prev->next = cur;
    retutrn dummy.next;
  }
};
```