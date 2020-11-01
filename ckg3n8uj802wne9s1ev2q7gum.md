## Present a modal with a custom view (ie. present a view controller using a xib)

These are the articles that helped me:
*  [No view is set](https://stackoverflow.com/questions/4763519/loaded-nib-but-the-view-outlet-was-not-set)

* [More than one view](https://stackoverflow.com/questions/13357788/a-view-can-only-be-associated-with-at-most-one-view-controller-at-a-time-uisegm)

The nitty gritty step by step details are in  [this blog post](https://coderlyn.hashnode.dev/when-titlelabeltext-hi-crashes-your-app-ckc4cfo0g00bd1gs1gvjp6djn).
 
Key steps:

* Add a xib file and a `.swift` file with the same name
* Make sure the xib file has a UIView and not a UIView Controller. If you have a view controller in the xib, then you will get the error: `A view can only be associated with at most one view controller at a time!`
* In the xib file, define the custom class for the file owner, not the view
* Prior to Xcode 12: In the xib file, click on file owner, then outlets, then drag and drop the view outlet to the view. If you don't do this, you will get the error: `loadViewFromNibNamed:bundle: loaded the nib but the view outlet was not set.`
* Xcode 12: In the xib file, click on file owner, then outlets, then drag and drop all the outlets as required from the right hand nav to the correct UI element in the nib. e.g. connect the label to the label, the button to the button.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602332229109/nGnVmpraK.png)

Don't forget you need initialisers in the `.swift` file. If you forget this step, then you will see a blank view. Nothing will crash, no warnings, but you won't see your custom view without initialisers.