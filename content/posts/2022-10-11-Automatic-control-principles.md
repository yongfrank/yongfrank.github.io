---
categories: study
date: "2022-10-11T16:37:00Z"
title: Automatic Control Principles
---

<!--
 * @Author: Frank Chu
 * @Date: 2022-10-11 18:41:40
 * @LastEditors: Frank Chu
 * @LastEditTime: 2022-10-11 18:55:03
 * @FilePath: /blog/_posts/2022-10-11-Automatic-control-principles.md
 * @Description: 
 * 
 * Copyright (c) 2022 by Frank Chu, All Rights Reserved. 
-->

## Mason's Gain Formula

[Wikipedia](https://en.wikipedia.org/wiki/Mason%27s_gain_formula)

Mason's gain formula (MGF) is a method for finding the [transfer function](https://en.wikipedia.org/wiki/Transfer_function) $H(s)$ of a linear signal-flow graph (SFG).

$$
P = \frac{\sum_{k=1}^n G_k \Delta_k}{\Delta} \\
\Delta = 1 - \sum L_i + \sum L_iL_j - \sum L_i L_j L_k + ... + (-1)^m\sum...+...
$$
