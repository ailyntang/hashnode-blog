## Debugging tips



### Visual debugger - how to find a particular constraint, or view, using its address

This [SO article](https://stackoverflow.com/a/47018422) has hit the nail on the head.

Look at the debugger to get the address, e.g.:
```
    "<NSLayoutConstraint:0x6000016e1220 UIView:0x7f7b32b9b040.height == 20   (active)>",
    "<NSLayoutConstraint:0x600001601c20 
```

Then go to the bottom left to paste that address into the search.

Note, sometimes there are no results. This can happen if the view was on the screen, but no longer is, because it's already been removed / re-rendered.

This happened for me where the loading views had conflicting constraints. So those loading views were only temporary.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1646693645774/isj8NkIi1.png)
