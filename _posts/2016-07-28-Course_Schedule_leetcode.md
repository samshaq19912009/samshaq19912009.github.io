---
layout: post
title: Course Schedule
date: 2016-08-02
categories: blog
tags: [Leetcode, Array]
description: Leetcode Solution
---

[Leetcode Link](https://leetcode.com/problems/course-schedule-ii/)

[Wiki page about Topological Sorting](https://en.wikipedia.org/wiki/Topological_sorting)


### Solution 1 : Topological Sort

```Python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        degrees = [0] * numCourses
        graph = [ [] for x in range(numCourses)]
        for node, pre in prerequisites:
            degrees[node] += 1
            graph[pre].append(node)
        courses = set(range(numCourses))
        flag = True
        while flag and len(courses):
            flag = False
            remove = []
            for x in courses:
                if degrees[x] == 0:
                    for node in graph[x]:
                        degrees[node] -= 1
                    remove.append(x)
                    flag = True
            for x in remove:
                courses.discard(x)
        return len(courses) == 0

```


### Solution 2: Depth First Search


```Python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
        """
        graph = [ [] for x in range(numCourses)]
        visited = [0] * numCourses
        
        for course, pre in prerequisites:
            graph[course].append(pre)
            
        for i in range(numCourses):
            if not self.dfs(i, visited, graph):
                return False
        return True
    
    def dfs(self, i, visited, graph):
        if visited[i] == -1:
            return False
        elif visited[i] == 1:
            return True
        visited[i] = -1
        for j in graph[i]:
            if not self.dfs(j, visited, graph):
                return False
        visited[i] = 1
        return True


```