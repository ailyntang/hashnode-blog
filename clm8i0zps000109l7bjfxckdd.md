---
title: "Objc.io: advanced alignment part 1"
datePublished: Thu Sep 07 2023 01:37:46 GMT+0000 (Coordinated Universal Time)
cuid: clm8i0zps000109l7bjfxckdd
slug: objcio-advanced-alignment-part-1
tags: swiftui

---

[https://talk.objc.io/episodes/S01E291-advanced-alignment-part-1](https://talk.objc.io/episodes/S01E291-advanced-alignment-part-1)

What did I learn?

* I kinda zoned out here
    
* Nice use of generics - good to get more familiar with these
    
* Noted that `@ViewBuilder` is a concept, which I have never seen before
    
* `ForEach` and the identifier - they did it two ways
    
    * `struct Tree<A>: Identifiable`
        
        * And then have `let id = UUID()` inside the struct
            
        * This is the proper way
            
    * `ForEach(thing, UUID: \.0) { }`
        
        * I haven't looked into the `\.` assignment shortcut yet
            
        * I think in this instance, maybe it works with `struct Thing` which does not conform to `Identifiable`