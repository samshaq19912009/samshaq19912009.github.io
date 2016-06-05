---
layout: post
title: Optimal Cashing 
date: 2016-06-05
categories: blog
tags: [Algorithms,Notes]
description: Optimal Cashing
---

In this programming problem and the next you'll code up the greedy algorithms from lecture for minimizing the weighted sum of completion times.. Download the text file here. This file describes a set of jobs with positive and integral weights and lengths. It has the format

number of jobs

job 1 weight job 1 length

job 2 weight job 2 length

```Python
## Read the data first

data = {}
lines = 0
for l in f.readlines():
    if lines == 0:
        lines += 1
        continue
    #weight length
    arr = l.strip().split(" ")
    data[lines] = [int(arr[0]), int(arr[1])]
    lines += 1
f.close()

##Add the difference by setting weight - length

for key in data:
    weight = data[key][0]
    length = data[key][1]
    diff = weight - length
    data[key].append(diff)
    
 
    
    
## Sort by difference break the tie by higher weight


sorted_data = sorted(data.items(), key = lambda x : (-x[1][2], -x[1][0]))


## Get the weight array and length array

length_array = [i[1][1] for i in sorted_data]
weight_array = [i[1][0] for i in sorted_data]

## Compute the final time arrary for the job list

final_length = [length_array[0]]
for i in range(1, len(length_array)):
    final_length.append(final_length[i-1] + length_array[i])
    
## Compute the final time needed


time = []
for i in range(0, len(length_array)):
    time.append(final_length[i]*weight_array[i])
sum(time)

## The best way is sort by the ratio weight/length

for key in data:
    weight = data[key][0]
    length = data[key][1]
    data[key].append(float(weight) / float(length))

sorted_data = sorted(data.items(), key = lambda x : (-x[1][2]))




```
