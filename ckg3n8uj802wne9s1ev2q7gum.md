## Present a view controller using a xib

These are the articles that helped me:
*  [No view is set](https://stackoverflow.com/questions/4763519/loaded-nib-but-the-view-outlet-was-not-set)

* [More than one view](https://stackoverflow.com/questions/13357788/a-view-can-only-be-associated-with-at-most-one-view-controller-at-a-time-uisegm)

Key steps:

* Add a xib file
* Make sure the xib file has a UIView and not a UIView Controller. If you have a view controller in the xib, then you will get the error: `A view can only be associated with at most one view controller at a time!`
* In the xib file, define the custom class for the file owner, not the view
* In the xib file, click on file owner, then outlets, then drag and drop the view outlet to the view. If you don't do this, you will get the error: `loadViewFromNibNamed:bundle: loaded thenib but the view outlet was not set.`


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602332229109/nGnVmpraK.png)
