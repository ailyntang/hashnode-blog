---
title: "View is mysteriously empty"
datePublished: Mon Sep 11 2023 03:05:01 GMT+0000 (Coordinated Universal Time)
cuid: clmeawln0000608ml0ac882pn
slug: view-is-mysteriously-empty
tags: swift

---

If you can't see your `view: UIView`, here is a quick checklist to go through:

* Set `view.translatesAutoresizingMaskIntoConstraints = false`
    
* Set and activate all FOUR constraints (top, bottom, trailing, leading)
    
* Ensure `view.isHidden = false`