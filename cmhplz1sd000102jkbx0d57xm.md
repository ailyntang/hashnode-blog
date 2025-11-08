---
title: "leetcode: 2 sum - think about DICTIONARIES"
datePublished: Sat Nov 08 2025 01:31:11 GMT+0000 (Coordinated Universal Time)
cuid: cmhplz1sd000102jkbx0d57xm
slug: leetcode-2-sum-think-about-dictionaries
tags: leetcode

---

Things I learnt:

### **When there is mapping to be done, think about a** `DICTIONARY`

* Here, we have to use the VALUES of the array, e.g. \[1, 2, 3, 4\]
    
    * Figure out which numbers add up to 6 = 2 + 4
        
* But we want to output the INDEX of the array
    
* So we need to map the values, 2 and 4, to their indices
    
* Whenever mapping is needed, think of using a DICTIONARY
    

### When thinking about `for` loops and lots of iterations, think about a `DICTIONARY`

Brute force:

* I knew I’d need to write some for loops and call stuff again and again til I got the solution.
    
* I also knew this would be poorly optimized for big O notation (time and space)
    
* I thought I’d optimised it because I was reducing the size of the array each time, but, even still, it wasn’t great apparently.