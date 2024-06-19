---
title: "Different types of caching"
datePublished: Wed Jun 19 2024 05:44:03 GMT+0000 (Coordinated Universal Time)
cuid: clxlercxs000f09mj7dy66ask
slug: different-types-of-caching
tags: dotnet

---

* **Read through**
    
    * When you read something, it checks the cache to see if it is there
        
    * If it's not in the cache, it will put it in the cache
        
    * This means data will always exist in the cache, unless the data doesn't exist
        
* **Write through**
    
    * When you write to the database, it will also write it to the cache
        
    * Also known as write aside caching
        
* **Write behind**
    
    * When you write to the cache, the cache knows how to persist the data to the database
        

Azure only supports write through caching