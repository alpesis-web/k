---
layout: post_tech
title: "Solving A Technical Question"
date: 2016-01-14 15:24:40 +0800
comments: true
categories: [tech]
tags: [programming, algorithm]
toc: false
---

Source: Cracking the Coding Interview

5 Steps

1. Ask questions.
2. Design a algorithm.
3. Pseudocode
4. Code
5. Test

## 1. Ask Questions

- What are the data types?
- How much data is there?
- What assuptions do you need to solve the problem?
- WHo is the user?

## 2. Design a algorithm

- What are its space and time complexity?
- What happens if there is a lot of data?
- Does your design cause other issues? For example, if you're creating a modified version of a binary search tree, did your design impact the time for insert, find, or delete?
- If there are other issues or limitations, did you make the right trade-offs? For which scenarios might the trade-off be less optimal?
- If they gave you specific data (e.g., mentioned that the data is ages, or in sorted order), have you leveraged that information? Usually there's a reason that an inter- viewer gave you specific information.

approaches

- examplify
- pattern matching
- simplify and generalize
- base case and build
- data structure brainstorm

## 3. Pseudocode

## 4. Code

- correct: expected and unexpected inputs
- efficient: time and space
- simple
- readable
- maintainable

## 5. Test

- Extremecases: 0, negative, null, maximums, minimums.
- User error:What happens if the user passes in null or a negative value?
- General cases: Test the normal case.
