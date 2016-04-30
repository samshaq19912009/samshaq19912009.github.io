---
layout: post
title: Largest Rectangle in Histogram
date: 2016-04-30
categories: blog
tags: [Leetcode,Notes]
description: Largest Rectangle in Histogram


---

``` cpp
class Solution {
public:
	int largestRect(vector<int> &height) {
		stack<int> s;
		int result = 0;
		height.push_back(0);
		for(int i=0; i<height.size(); ) {
			if(s.empty() || height[i] > height[s.top()])
				s.push(i++)
			else {
				int tmp = s.top();
				s.pop();
				result = max(result, height[tmp] * (s.empty()? i : i-s.top() - 1);
				}
		}
		return result;
	}
};
		


```