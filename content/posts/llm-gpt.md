---
title: "LLM GPT"
date: 2023-04-28T09:02:54+08:00
draft: true
---

## 如何将单词转换为向量

在自然语言处理（NLP）中，我们需要一种方法来让计算机理解和处理文本数据。要做到这一点，我们通常会将单词转换为数值表示，即向量。

一个简单的方法是使用 "one-hot encoding"。想象一下，我们有一个词汇表，其中包含四个单词：苹果、香蕉、橙子和葡萄。我们可以为每个单词创建一个向量，向量的长度等于词汇表中的单词数量。在这个例子中，长度为 4。然后，我们在与单词对应的位置上放置 1，其他位置放置 0。

例如：

苹果：[1, 0, 0, 0]
香蕉：[0, 1, 0, 0]
橙子：[0, 0, 1, 0]
葡萄：[0, 0, 0, 1]
然而，这种方法在大型词汇表中效果不佳，因为向量会变得非常大而稀疏。

因此，我们使用一种称为 "word embedding" 的方法，它可以更有效地表示单词。Word embedding 是一种将单词映射到固定长度的向量的技术，这些向量可以捕捉单词之间的相似性。常见的 word embedding 方法有 Word2Vec 和 GloVe。

Word2Vec 和 GloVe 都是预先训练好的模型，它们已经学会了从大量文本数据中捕捉单词的相似性。这些模型将单词转换为更短的向量（如 50、100 或 300 维），并且语义相似的单词在向量空间中距离更近。

例如，使用 Word2Vec 或 GloVe，我们可能会得到类似以下的向量表示：

苹果：[0.24, -0.12, 0.45, ..., -0.32]
香蕉：[0.22, -0.08, 0.51, ..., -0.27]
通过将单词转换为向量，我们可以让计算机更好地理解和处理文本数据。