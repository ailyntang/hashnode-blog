---
title: "Objc.io - SwiftUI state explained"
datePublished: Mon Sep 11 2023 02:37:12 GMT+0000 (Coordinated Universal Time)
cuid: clme9wu4i000a09mkgvk4b9ub
slug: objcio-swiftui-state-explained
tags: swiftui

---

# Episode 1: setup the groundwork

[https://talk.objc.io/episodes/S01E261-views-and-nodes](https://talk.objc.io/episodes/S01E261-views-and-nodes)

What is covered

* There are 8 videos. Goal is to recreate SwiftUI using `import Foundation` only. That way we can see what `import SwiftUI` is doing under the hood to update only the views that are dirty, and no other view
    
* This episode lays the groundwork for various protocols. It doesn't actually build any views (I don't know if it will)
    
* The goal is to have a button title update from 0, 1, 2, etc each time the button is tapped
    

What I learnt

* This kind of video is not my jam. I have to really pay attention, otherwise I drift
    
* I prefer videos which are less abstract, and more about using `SwiftUI` to demonstrate what `SwiftUI` does. Rather than videos which build something else.
    

# Episode 2: build observable / observed relationship

[https://talk.objc.io/episodes/S01E262-observed-objects](https://talk.objc.io/episodes/S01E262-observed-objects)

This is getting more interesting / useful for my purposes.

Specifically: `let cancellable = view.objectWillChange.sink { ... }` at 9:00 of the video

Things they discussed that seem very relevant to me:

> The `cancellable` must be stored in order to keep the subscription alive

* I assume the subscription is the observer. So if that's not kept alive, then if the object changes, no one will be notified of its change.
    

> An observed object will never be passed around. It will always be the property of a single view.
> 
> If you want to observe the same object multiple times, you will need to create multiple observed objects.

hmm. Ok.

So let's say I have a modal with two boxes.

The user can select one of the two boxes, and I want the UI to update:

* On the modal: the box that is tapped gets outlined; the other box is no longer outlined
    
* On the underlying page that presented the modal: this displays the user's modal selection
    

Then do we have TWO observed objects?

Box one AND box two?

For the underlying page, it observes when box one is tapped, then it sends a signal and the user's selection is displayed.

* This makes sense to me
    

But.. what about on the modal?

So Box one is observed by box two- and then if box one is tapped, box two updates itself.

And vice versa.

So the relationship is:

* `BoxOne: ObservedObject`
    
    * Observed by `BoxTwo` and `UsersModalSelection`
        
* `BoxTwo: ObservedObject`
    
    * Observed by `BoxOne` and `UsersModalSelection`
        

So then.. we have something like:

```plaintext

let cancellable = boxOne.objectWillChange.sink { _ in
   // tell observers? 
   // Or is that what `objectWillChange` does?
}
```

# Episode 3: TupleView

Now they try to see if they can replicate the SwiftUI functionality with a nested view.

This video sets up the start of the code for that, but doesn't get the full way there.

* First they copy and paste their code, built with only the Foundation api, into an app which is running the SwiftUI api
    
* We see that yes, it works as expected
    
* Then, they setup tests and code to test the nested views
    
* As part of this setup, they refactor the code with a `@resultsBuilder`.
    
    * This is the interesting part to me - as I have seen Ryan do this with `@CellsBuilder` . Now I know where it's from!
        
    * Originally this was a `@_functionBuilder` that was replaced with `@resultsBuilder` when they added a second function