## Stackviews and labels that need to wrap

All images are from: https://useyourloaf.com/blog/stack-views-and-multi-line-labels/

Sometimes I want this UI, where a label wraps, and there's something else on the right hand side that should keep its size.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627345942664/6Gq8TJwop.png)

Ideally I'd put the label and the switch into a horizontal stack view to do this.

However when I do this, this occurs:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627346041745/fW6IFVrXP.png)

This article says it's not possible to do this with stackviews.
The article is wrong now.

You need to update these things to get the UI I want:
* On the newly created horizontal stackview
  * Alignment: center
  * Distribution: fill proportionally
  * Then set the normal constraints for this stackview to the content view (anchors to top, bottom, leading, trailing etc)

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627346129870/-x5CbtAYN.png)

* On the right hand side item that you want to be as small as possible:
  * Content hugging priority = 1000
    * 1000, not just 751, because 1000 means required, which is what we want


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627346175085/zXA6XQVgd.png)
