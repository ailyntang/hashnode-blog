---
title: "Stackview woes"
datePublished: Mon Apr 29 2024 06:20:50 GMT+0000 (Coordinated Universal Time)
cuid: clvkkm7g9000b0al75una51nv
slug: stackview-woes
tags: swift

---

Compression resistance and content hugging woes.

Sigh.

Some things to remember

** Constraints trump CRP and CHP**
* As in, if you have set a height and width, then that element won't try to grow or shrink, irrespective of its CRP and CHP
* Constraints have an automatic priority of 1000. That's why they trump CRP and CHP. If you update that priority.. then bets are off.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651817682967/Lo3f0pZxh.png align="left")

**CRP and CHP of elements INSIDE the stackview are important**

Situation
* You have two labels in a vertical stackview
* These labels, in turn, are in a horizontal stackview
* If you want your labels to take up lots of space, or to hug tight, then you have to update the CRP and CHP of the label itself.
  * updating the CRP / CHP of the vertical stackview will NOT make the change you want

