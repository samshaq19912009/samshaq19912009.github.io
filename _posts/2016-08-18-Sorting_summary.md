---
layout: post
title: Sorting Summary
date: 2016-08-18
categories: blog
tags: [algorithm, sorting]
description: Sorting summary
---








### Selction Sort

核心：不断地选择剩余元素中的最小者。
找到数组中最小元素并将其和数组第一个元素交换位置。
在剩下的元素中找到最小元素并将其与数组第二个元素交换，直至整个数组排序。
性质：

* 比较次数=(N-1)+(N-2)+(N-3)+...+2+1~N^2/2
 
* 交换次数=N

* 运行时间与输入无关

* 数据移动最少

```
    def sortIntegers(self, A):
        # Write your code here

        n = len(A)
        for i in range(n):
            min_index = i
            for j in range(i+1,n):
                if A[j] < A[min_index]:
                    min_index = j
                   
            A[i], A[min_index] = A[min_index], A[i]
        return A

```

### Bubble Sort

核心：冒泡，持续比较相邻元素，大的挪到后面，因此大的会逐步往后挪，故称之为冒泡

```
    def sortIntegers(self, A):
        # Write your code here

        n = len(A)
        for i in range(n):
            for j in range(1,n-i):
                if A[j-1] > A[j]:
                    A[j], A[j-1] = A[j-1], A[j]
        return A
                
```

### Insertion Sort

核心：通过构建有序序列，对于未排序序列，在已排序序列中从后向前扫描(对于单向链表则只能从前往后遍历)，找到相应位置并插入。实现上通常使用in-place排序(需用到O(1)的额外空间)

从第一个元素开始，该元素可认为已排序
取下一个元素，对已排序数组从后往前扫描
若从排序数组中取出的元素大于新元素，则移至下一位置
重复步骤3，直至找到已排序元素小于或等于新元素的位置
插入新元素至该位置
重复2~5
性质：

* 交换操作和数组中倒置的数量相同

* 比较次数>=倒置数量，<=倒置的数量加上数组的大小减一

* 每次交换都改变了两个顺序颠倒的元素的位置，即减少了一对倒置，倒置数量为0时即完成排序。

* 每次交换对应着一次比较，且1到N-1之间的每个i都可能需要一次额外的记录(a[i]未到达数组左端时)

* 最坏情况下需要~N^2/2次比较和~N^2/2次交换，最好情况下需要N-1次比较和0次交换。

* 平均情况下需要~N^2/4次比较和~N^2/4次交换

```
    def sortIntegers(self, A):
        # Write your code here
        for i in range(1,len(A)):
            index = i
            tmp = A[i]
            while index > 0 and A[index-1] > tmp:
                A[index] = A[index-1]
                index -= 1
            A[index] = tmp


        return A
                
```