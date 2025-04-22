---
layout: post
title: Binary Search
description: Notes
permalink: /binary-search-notes
--- 
# Binary Search

## What is Binary Search
An algorithm that allows you to divide and conquer to search for a value within a set.

## How it Works
- The list or array needs to be sorted before the algorithm starts.  
- In the actual algorithm: set two bounds, lowest and highest.  
- Looping from high to low, always trying to find the middle integer.  
- The goal is to split the area we're searching in half each time.

## Best Case for Binary Search
The best case occurs if you find the wanted value immediately.

## Implementing in Python
- Define a function to take two parameters.

## Why is Binary Search So Fast
- Cuts the search space in half at every step.  
- The divide and conquer strategy dramatically reduces the number of checks.

## Big O Notation
Big O notation describes the worst-case performance of an algorithm in terms of input size (n).

## Practical Applications of Binary Search
- Databases and indexing  
- Computer science competitions  
- Games (AI Pathfinding)  
- Spell checkers  
- Networking  
- Search algorithms in libraries  
- Finding closest values  

## Binary Search with Strings
A list of strings must be sorted alphabetically or lexicographically.
- Alphabetical: Strings are arranged like words in a dictionary.
- Capital letters matter: Uppercase letters come before lowercase.

# <a href="{{ site.baseurl }}/binary-search">Link to Popcorn Hacks and Homework</a>