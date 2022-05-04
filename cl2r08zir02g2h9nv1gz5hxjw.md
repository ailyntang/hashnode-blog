## UIButton - font will not update

**PSA: UIButton and interface builder - use default style to update fonts**

**TL;DR: **Update the button style to Default instead of Plain

**Problem**
- If you add a UIButton via interface builder, then try to use button.apply(style: .secondary) , you may notice the font doesn’t update sometimes.
- The font looks like it is applied when you check with the debugger and type `po button.titleLabel?.font`
- But.. don’t be fooled. It is not applied at all.

**Fix**
- Update the button style to `Default` instead of `Plain`
- Note that when you add a button in interface builder, the style is automatically set to plain
- We assume that in one of the new Xcodes, the plain style of button no longer lets you apply a font via `button.titleLabel?.font`. Somehow whatever value that is stored in there is over-written back to the Plain font.
- The documentation doesn’t mention this oddity at all :woman-shrugging:



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651633981291/c8bMe9Wsj.png align="left")


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651633993039/9UkLddM_L.png align="left")
