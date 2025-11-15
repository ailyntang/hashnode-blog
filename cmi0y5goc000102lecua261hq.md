---
title: "leetcode: merge strings alternately"
datePublished: Sat Nov 15 2025 23:57:33 GMT+0000 (Coordinated Universal Time)
cuid: cmi0y5goc000102lecua261hq
slug: leetcode-merge-strings-alternately
tags: swift, leetcode

---

[https://leetcode.com/problems/merge-strings-alternately/editorial/?source=submission-ac](https://leetcode.com/problems/merge-strings-alternately/editorial/?source=submission-ac)

### In swift, you cannot access characters like `someString[3]`

* It’s a big song and dance with getting a string index from the startIndex
    
* Once you have the start index, it’s cheap to get the *next* index
    
* But, if you calculate a new index, or use `offsetBy` that’s expensive
    

### Alternatives to string index, to make life easier

* `for c in string`
    
* `var array = Array(string)`
    

### If you know the length of the output string, reserve capacity

* This will be more efficient
    

```plaintext
    func mergeAlternatelyOptimised(_ word1: String, _ word2: String) -> String {
        var output = ""
        output.reserveCapacity(word1.count + word2.count)

        var word1Index = word1.startIndex
        var word2Index = word2.startIndex

        while word1Index < word1.endIndex, word2Index < word2.endIndex {
            let char1 = word1[word1Index]
            output.append(char1)
            word1Index = word1.index(after: word1Index)

            let char2 = word1[word2Index]
            output.append(char2)
            word2Index = word2.index(after: word2Index)
        }


        if word1Index < word1.endIndex {
            output.append(contentsOf: word1[word1Index...])
        } else {
            output.append(contentsOf: word2[word2Index...])
        }

        return output
    }
```