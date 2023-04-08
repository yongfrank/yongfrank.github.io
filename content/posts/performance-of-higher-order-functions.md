---
title: "Performance of Higher Order Functions in Swift"
date: 2023-04-02T14:23:01+08:00
---

## Why do I compose this article?

* Leetcode algorithm problems
* Swift Language

## Article by skoumal

> [Performance of built-in higher-order functions Map, Filter, Reduce, and flatMap vs. for-in loop in Swift](https://www.skoumal.com/en/performance-of-built-in-higher-order-functions-map-filter-reduce-and-flatmap-vs-for-in-loop-in-swift/)
>
> The most popular higher-order functions are map, filter, and reduce. We all use them since we think that syntax is much better and it is even faster to write them than the old way for-in loop. But is it really true? Have you ever thought about the performance of these built-in functions? They are built-in so naturally, they should be better, right? ???? Let’s dive into these functions together and uncover the truth ????.
>
> According to test cases, there is nothing wrong with using high-order functions when we do NOT need to chain them. The performance is way better (1.63x faster) when we use built-in map function or slightly better/worse when we use built-in filter/reduce.
>
> If we want to chain high-order functions we should consider not using them and implementing them as a for-in loop solution. The performance is way better, 2.37x faster (in examples of this article) than built-in functions.
>
> There is no need to always use modern elements, structures, or functions of a modern language just because the syntax looks better or everyone uses it. Time and space complexity is way more important than modern syntax and in the end, the fancy syntax does not make us better developers, even though we might think it does.
