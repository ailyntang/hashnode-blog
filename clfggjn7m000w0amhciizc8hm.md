---
title: "[LayoutConstraints] Unable to simultaneously satisfy constraints"
datePublished: Mon Mar 20 2023 06:40:30 GMT+0000 (Coordinated Universal Time)
cuid: clfggjn7m000w0amhciizc8hm
slug: layoutconstraints-unable-to-simultaneously-satisfy-constraints
tags: swift

---

```plaintext
[LayoutConstraints] Unable to simultaneously satisfy constraints.
	Probably at least one of the constraints in the following list is one you don't want. 
	Try this: 
		(1) look at each constraint and try to figure out which you don't expect; 
		(2) find the code that added the unwanted constraint or constraints and fix it. 
(
    "<NSLayoutConstraint:0x600000071d10 UIView:0x1555f3420.height >= 330   (active)>",
    "<NSLayoutConstraint:0x6000000733e0 V:|-(10)-[UILabel:0x1555f30f0]   (active, names: '|':UIView:0x1555f2f40 )>",
    "<NSLayoutConstraint:0x6000000734d0 V:[UILabel:0x1555f30f0]-(24)-[UIView:0x1555f3420]   (active)>",
    "<NSLayoutConstraint:0x600000073520 V:[UIView:0x1555f3420]-(16)-[UIPageControl:0x1555f35d0]   (active)>",
    "<NSLayoutConstraint:0x600000073610 V:[UIPageControl:0x1555f35d0]-(16)-|   (active, names: '|':UIView:0x1555f2f40 )>",
    "<NSLayoutConstraint:0x6000000737a0 V:|-(0)-[UIView:0x1555f2f40]   (active, names: '|':UITableViewCellContentView:0x1555ed190 )>",
    "<NSLayoutConstraint:0x6000000737f0 V:[UIView:0x1555f2f40]-(0)-|   (active, names: '|':UITableViewCellContentView:0x1555ed190 )>",
    "<NSLayoutConstraint:0x600000078460 'UIView-Encapsulated-Layout-Height' UITableViewCellContentView:0x1555ed190.height == 380   (active)>"
)

Will attempt to recover by breaking constraint 
<NSLayoutConstraint:0x600000071d10 UIView:0x1555f3420.height >= 330   (active)>

Make a symbolic breakpoint at UIViewAlertForUnsatisfiableConstraints to catch this in the debugger.
The methods in the UIConstraintBasedLayoutDebugging category on UIView listed in <UIKitCore/UIView.h> may also be helpful.
```

### **Why does this note appear? My code compiles fine.**

* It appears when Xcode realises there are multiple constraints on a view, and it can't fulfil all of them as instructed
    
* Usually, the app compiles and runs fine. Even in the visual debugger, there is no issue
    
    * I assume this is because Xcode has already resolved the conflict at runtime, so that there no longer is a conflict when you open the visual debugger
        

### **How do I fix the conflicts?**

Usually, you only need to fix ONE of the conflicts, not all.

* Most scenarios resolve when you lower the priority of one of the constraints
    
* The trick is in figuring out which one you need to lower
    
* Once you figure that out, all the other constraint conflicts typically go away
    

This is how to read the conflicts:

* `V:|-(10)-[UILabel:0x1555f30f0]`
    
    * `V`: this means Vertical
        
    * So this constraint means there is a `UILabel` with a padding of `10` between the top of the label and the `topAnchor` of its superview
        
    * `0x1555f30f0`: Open the visual debugger, and then down the bottom, paste this into the filter. That will show you the correct `UILabel`
        
* `'UIView-Encapsulated-Layout-Height' UITableViewCellContentView:0x1555ed190.height == 380`
    
    * The view with conflicts uses a `UITableView`
        
    * Somewhere in your app, perhaps programmatically, the `tableView.estimatedRowHeight` has been set to `380`
        
    * You should override this value to the correct number (which will be a larger number).
        
    * In the example above, Xcode has said the row height can be greater than 380, and that has fixed the issue.
        

### **When I've fixed the constraint, why does the note never go away?**

When you see constraint conflicts like this, note that the debug console will only show ONE set of conflicting constraints at a time.

* If you have a view with MULTIPLE constraint issues, the debugger will only complain about ONE. And it's always the same one.
    
* Once you FIX this, the NEXT constraint conflict will appear
    
* This can drive you mad if you don't realise the new conflict is totally different to the one you just fixed