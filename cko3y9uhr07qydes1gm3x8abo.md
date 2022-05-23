## Stackviews and labels that need to wrap

*Images are from: https://useyourloaf.com/blog/stack-views-and-multi-line-labels/*

# Summary

* Set constraints on the STACKVIEWS, not on the elements inside the stackview
* Set content compression resistance and content hugging priorities on the LABELS, not  the stackview
* If you see an encapsulated layout height constraint that you don't want, check that your elements inside your stackview are not constrained to something outside the stackview (like the superview)

# 1. Setup your stackview correctly

The stackview should have these things set:
* standard spacing
* alignment
* distribution
* custom spacing, if needed
* constraints

```
let stackView = UIStackView()
stackView.axis = .vertical
stackView.alignment = .fill
stackView.spacing = 12.0
stackView.setCustomSpacing(20.0, after: imageView)
```

# 2. Apply Constraints

The stackview itself needs constraints. ie. you should constraint the stackview's top, bottom, leading and trailing anchors, relative to other items. E.g. you can constrain them to the super view with zero padding.

However elements INSIDE a stackview should not have any constraints (except for height and width, e.g. for images.). The stackview should manage padding between each element inside the stackview, through the settings in section 1.


### Do not constrain items INSIDE the stackview to something OUTSIDE the stackview

If you do this, the compiler will not complain.
However, you'll likely get a bug when your label is longer than expected.

The bug manifests as an unexpected constraint. E.g.:
* My label keeps truncating
* My label won't wrap
* My stackview height is fixed and I didn't do it
* I keep seeing an enscapulated layout height constraint that I didn't set anywhere, and providing an estimation for the table view cell height does not fix this

Example
* You have an image view of an icon inside the stackview, and a label
* Stackview is constrained to its superview's top, leading and trailing anchors, with 0 padding
  * This is a required constraint to supply - you need to tell autolayout where to put the stackview.
  * Bottom anchor is not fixed, so that the stackview height can grow based on the label inside

* Icon has a fixed height and width of 24px (this is allowed, constraints are internal to itself)
* Icon top anchor is constrained to superview top anchor, with a padding of 10px (This is the example of what NOT to do)
  * Now the height of your view will always be 24px + 10px + stackview to superview bottom padding (let's say that's 0px)
  * So your view height will always be 34px. It will never be taller. No matter how hard you try.
  * In the visual debugger, this constraint comes up as the `encapsulated layout height`. The `encapsulated layout height` is used under the hood by the layout engine - it's the result of the autosizing pass.

**The fix**

Delete the icon top anchor constraint.
It shouldn't be there.
If you need a top anchor constraint, set the stackview top anchor constraint. 
Or if you need something more custom, play around with the stackview settings (see section 1).

These will likely get you MOST of the way there.

If you are still having trouble with wrapping labels, then you need to look at your content hugging and compression resistance priorities.


# 3. Set Content Hugging & Compression Resistance Priorities

Ok.. so let's say you have stackviews within stackviews in a view, like so:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653265482226/_T3yap7Pd.png align="left")


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653265493081/yqtGBacHv.png align="left")

This view is made up of a container horizontal stackview.
The container stackview has two vertical stackviews (for label and description).

In this instance, you need to set the content hugging priority and content compression resistance priorities of the LABELS in order to get the correct behaviour.

Note.. it's the priorities of the LABELs, and NOT the priorities of the stackview.


# 4. Priorities, priorities

If you don't play around with anchor constraint priorities, then these are set to 1000.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653266188739/0sh06tIdi.png align="left")


Swift will enforce constraints first, before looking at CHP and CCRP.

For example - the icon height and width of 24px will be enforced before the icon's CHP and CCRP values.

However, you can change the anchor constraint priority.
If you do this.. then.. erm.. I'm not sure how Swift determines which to look at first.
Cos CHP value of 1000.. I don't think that means "CHP priority over Anchor Constraint Priority of 1000". If you get my drift.

Sooo, if you are getting weird bugs, just CHECK that your anchor priorities are at 1000.
I can't see a use case for when they should be lower than that.



# Original write up

Sometimes I want this UI, where a label wraps, and there's something else on the right hand side that should keep its size.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627345942664/6Gq8TJwop.png)

Ideally I'd put the label and the switch into a horizontal stack view to do this.

However when I do this, this occurs:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627346041745/fW6IFVrXP.png)

This article says it's not possible to do this with stackviews.
The article is wrong.

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



