## Maps and locations

**Launch location permission modal**

* Add the relevant `Privacy - Location ...` option to the `Info.plist`. You do not need to add more than one option, contrary to stack overflow.
* Ensure you are using the correct `Info.plist`. There can be multiple in the project. The one you want is under your App Target.

**Mapview**

You need to set the delegate for the mapview.
If you haven't, then when you try to update the map view, nothing will happen and you'll see this message in the debug console: `[Client] {"msg":"#NullIsland Either the latitude or longitude was exactly 0! That's highly unlikely", "latIsZero":0, "lonIsZero":0}  `

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1619157170315/HloV_mRqf.png)
