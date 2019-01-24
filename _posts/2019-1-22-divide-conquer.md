---
layout: post
title: divide and conquer
categories: [algorithms]
tags: [algorithms]
fullview: false
comments: false
---

#### Format of Divide-and-Conquer algorithms

- Divide: Split the array or list into smaller pieces
- Conquer: Solve the same problem recursively on smaller pieces
- Combine: Build the full solution from the recursive

### Master Recurrence Theorem (simpeler version)
	If T(n) = a T(n/b) + θ(n^k) then the solution is:
	T(n) = θ(n^k) if logb a < k (or a < b^k)
	T(n) = θ(n^k lg n) if logb a = k (or a = b^k)
	T(n) = θ(n^logb a) if logb a > k (or a > b^k)

[redirect to class materia](http://cs470.cs.ua.edu/fall2017/schedule.htm)
