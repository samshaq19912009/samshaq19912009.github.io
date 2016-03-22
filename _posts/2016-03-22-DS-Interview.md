---
layout: post
title: Data Science Interview Question
date: 2016-03-22
categories: blog
tags: [Machine Learning,知识管理]
description: 
---


1. Given a coin you don’t know it’s fair or unfair. Throw it 6 times and 
get 1 tail and 5 head. Determine whether it’s fair or not. What’s your 
confidence value? 

2. Given Amazon data, how to predict which users are going to be top 
shoppers in this holiday season. 

3. Which regression methods are you familiar? How to evaluate regression 
result? 

4. Write down the formula for logistic regression. How to determine the 
coefficients given the data? 

5. How do you evaluate regression? 
For example, in this particular case:
item click-through-rate  predicted rate
1       0.04        0.06
2       0.68        0.78
3       0.27        0.19
4       0.52        0.57
…

6. What’s the formula for SVM? What is decision boundary? 

7. A field with unknown number of rabbits. Catch 100 rabbits and put a label
on each of them. A few days later, catch 300 rabbits and found 60 with 
labels. Estimate how many rabbits are there?  

8. Given 10 coins with 1 unfair coin and 9 fair coins. The unfair coin has &
#8532; prob. to be head. Now random select 1 coin and throw it 3 times. You 
observe head, head, tail. What’s the probability that the selected coin is 
the unfair one? 

9. What’s the formula for Naive Bayesian classifier? What’s the assumption
in the formula? What kind of data is Naive Bayesian good at? What is not? 

10. What is the real distribution of click-through rate of items? If you 
want to build a predictor/classifier for this data, how do you do it? How do
you divide the data? 

11. You have a stream of data coming in, in the format as the following:
item_id, views, clicks, time
1            100     10         2013-11-28
1            1000   350       2013-11-29
1            200     14         2013-11-30
2            127     13         2013-12-1
…

The same id are consecutive. 

Click through rate = clicks / views. 
On every day, I want to output the item id when its click through rate is 
larger than a given threshold. 
For example, at day 1, item 1’s rate is 10/100=10%, day2, its (10+350)/(100
+1000)=0.32. day3 it is (10+350+14)/(100+1000+200)=0.28. 
If my threshold is 0.3, then at day 1, I don’t output. On day2 I output. On
day3, I don’t output.

11. Given a dictionary and a string. Write a function, if every word is in 
the dictionary return true, otherwise return false. 

12. Generate all the permutation of a string. 
For example, abc, acb, cba, … 

13. We want to add a new feature to our product. How to determine if people 
like it?
A/B testing. How to do A/B testing? How many ways? pros and cons? 

14. 44.3% vs 47.2% is it significant?  

15. Design a function to calculate people’s interest to a place against the
distance to the place.

16. How to encourage people to write more reviews on Yelp? How to determine 
who are likely to write reviews? How to increase the registration rate of 
Yelp? What features to add for a better Yelp app? We are expanding to other 
countries. Which country we should enter first? 

17. What’s the difference between classification and regression? 

18. Can you explain how decision tree works? How to build a decision tree 
from data? 

19. What is regularization in regression? Why do regularization? How to do 
regularization? 

20. What is gradient descent? stochastic gradient descent?

21. We have a database of <product_id, name, description, price>. When user 
inputs a product name, how to return results fast? 

22. If user gives a budget value, how to find the most expensive product 
under budget? Assume the data fits in memory. What data structure, or 
algorithm you use to find the product quickly? Write the program for it. 

23. Given yelp data, how to find top 10 restaurants in America?

24. Given a large file that we don’t know how many lines are there. It 
doesn’t fit into memory. We want to sample K lines from the file uniformly.
Write a program for it. 

25. How to determine if one advertisement is performing better than the 
other? 

26. How to evaluate classification result? What if the results are in 
probability mode? 
If I want to build a classifier, but the data is very unbalanced. I have a 
few positive samples but a lot of negative samples. What should I do?

27. Given a lot of data, I want to random sample 1% of them. How to do it 
efficiently? 

28. When a new user signs up Pinterest, we want to know its interests. We 
decide to show the user a few pins, 2 pins at a time. Let the user choose 
which pin s/he likes. After the user clicks on one of the 2, we select 
another 2 pins. 
Question: how to design the system and select the pins so that we can 
achieve our goal? 

29. Write a function to compute sqrt(X). Write a function to compute pow(x, 
n) [square root and power)


30. Given a matrix
a b c  d
e f  g  h
i  j  k   l
Print it in this order:
a  f  k
b g l
c h
d
e j
i

31. Given a matrix and an array of words, find if the words are in the 
matrix. You can search the 

matrix in all directions:  from left to right, right to left, up to down, 
down to up, or diagonally. 
For example
w o r x b
h  e l  o v
i   n d e m

then the word “world” is in the matrix. 


32. Given a coordinates, and two points A and B. How many ways to go from A 
to B? You can only move up or right. 
For example, from (1, 1) to (5, 7), one possible way is 1,1 -> 2, 1… 5, 1 -
> 5,2 -> ..5, 7


33. In a city where there are only vertical and horizontal streets. There 
are people on the cross point. These people want to meet. Please find a 
cross point to minimize the cost for all the people to move. 

34. Design a job search ranking algorithm on glassdoor

35. How to identify review spam?

36. Glassdoor has this kind of data about a job : (position, company, 
location, salary). For example (Software Engineer, Microsoft, Seattle, $125K
). For some records, all four entires are available. But for others, the 
salary is missing. Design a way to estimate salary for those records.

37. When to send emails to users in a day can get maximum click through rate?

38. Youtube has video play log like this:
Video ID, time
vid1        t1
vid2        t2
...           ...
The log is super large. 
Find out the top 10 played videos on youtube in a given week.

39. Write a program to copy a graph

40. A bank has this access log:
IP address, time
ip1      t1
ip2      t2
...        ...

If one ip accessed K times within m seconds, it may be an attack. 
Given the log, identify all IPs that may cause attack. 
