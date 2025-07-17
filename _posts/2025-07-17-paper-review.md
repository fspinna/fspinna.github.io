---
layout: post
title: "Learning Comprehensible Descriptions of Multivariate Time Series"
date: 2025-07-17
description: "A method for learning interpretable features from multivariate time series using Parametrized Event Primitives (PEPs)."
tags: [time series, classification, xai]
categories: [review]
giscus_comments: false
related_posts: false
---

I recently came across a paper from 1997 by Mohammed Waleed Kadous titled _"Learning Comprehensible Descriptions of Multivariate Time Series."_ It was truly ahead of its time, introducing a compelling way to turn high-dimensional temporal data into interpretable features, 10 years before shapelets were even proposed.

At the heart of the paper is a deceptively simple idea: instead of feeding entire time series into a learning algorithm, they extract **Parametrized Event Primitives**, or **PEPs**.

### What is a PEP?

A PEP is a template for detecting specific patterns in time series data. Each pattern is described by a set of parameters, for example, the **start time** and **duration** of a rising trend, a local maximum, or in the following example, just a sequence of `True` values.

You can think of a PEP like a filter: it scans a stream, finds all the events that match its pattern, and records their parameter values.

Here's a simple dataset from the paper’s toy example, called the "BR task." Each stream is a binary sequence (`T` = true, `F` = false), labeled either _Red_ or _Blue_:


|     | Stream                      | Class | Runs of T's (start, length) |
| --- | --------------------------- | ----- | --------------------------- |
| 1   | FFF**TTT**FFFFFF            | Red   | ${(3,3)}$                   |
| 2   | FFF**T**FF**T**FF**TTTT**   | Blue  | ${(3,1), (6,1), (9,4)}$     |
| 3   | FF**T**FF**T**FFFFFF**TTT** | Blue  | ${(2,1), (5,1), (12,3)}$    |
| 4   | FFFF**TTTT**FFFFF           | Red   | ${(4,4)}$                   |
| 5   | FFF**TTT**FFFF              | Red   | ${(3,3)}$                   |
| 6   | FF**TT**FF**T**FF**TTT**    | Blue  | ${(2,2), (6,1), (9,3)}$     |


Each stream is analyzed with the `TrueRun` PEP: it just finds the runs of `T` values and records their start and length.

### From Raw Events to Features

Once all these events are extracted, they plot them in a **parameter space**, where each point is a tuple like $(t, d)$ (start time and duration).
They then **cluster** this space. Each cluster groups together events that are similar in time and shape. 

![](/assets/img/posts/2025-07-17-paper-review_fig1.svg)

These clusters become new binary features for learning. If a stream contains an event from Cluster A, then `A = True`; otherwise, `A = False`. In our example:

|     | A     | B     | C     |
| --- | ----- | ----- | ----- |
| 1   | False | True  | False |
| 2   | True  | False | True  |
| 3   | True  | False | True  |
| 4   | False | True  | False |
| 5   | False | True  | False |
| 6   | False | False | True  |

This approach can transform complex time series into a simple table of features. And importantly, these features are **interpretable**, they correspond to understandable event patterns.

For example, Cluster C captures events characterized by a "_long and late true run_." Such patterns can be informative features for downstream models, as they often correlate with specific target classes. In this case, these long and late true runs occur exclusively in instances labeled "Blue," making them potentially strong indicators of that class.

```biblatex
@inproceedings{kadous1999learning,
  title={Learning Comprehensible Descriptions of Multivariate Time Series.},
  author={Kadous, Mohammed Waleed},
  booktitle={ICML},
  volume={454},
  pages={463},
  year={1999}
}
```
