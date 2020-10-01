## When titleLabel.text = "Hi" crashes your app

This happens when you are using storyboards and xibs.

Here are some reasons your app could crash:
* You forgot to connect the outlet to your `.swift` file
* You are calling `titleLabel.text = "xyz"` too early. It needs to be done in `viewDidLoad`, or sometime after the view is presented. It can't be called before the view is presented.
* You are instantiating a class instead of a nib (or vice versa)

### Creating a custom table view cell or collection view cell

Let's say you have a `CreateHabitHeaderView.xib` file that you want to use inside a collection view.

You'll have a corresponding `CreateHabitHeaderView.swift` file as well, which links up to that `.xib`.

Here's what you need to do in the `.xib`.
1. Set a custom class for the overall view (circled below in red). Currently my `.xib` is a collection view cell. Yours may be a view, or a table view cell, or something else. 


![Screen Shot 2020-09-30 at 11.25.55 am.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601429775672/1cnWgeJe9.png)

Open up the right hand nav in  Xcode. We want to set the custom class to `CreateHabitHeaderView`. This references your file `CreateHabitHeaderView.swift`.

Note that the name of the `.swift` and the `.xib` files must be exactly the same. If they are not, there will be issues.

Below is the default setting, where no custom class is specified.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601429263424/PdVYI36hD.png)

Once you start typing in your file name, it should prepopulate for you. Note that you don't need the `.swift` at the end.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601429361437/Smbrm6UD7.png)

Now your `.xib` is ready to use in other views. You will need to instantiate the `.xib` to use it. 

Given I have a collection view cell, I will need to register the nib by doing this:
```
collectionView.register(UINib(nibName: "CreateHabitHeaderView", bundle: nil), forSupplementaryViewOfKind: UICollectionView.elementKindSectionHeader, withReuseIdentifier: Text.sectionHeaderIdentifier)
```

Be careful that you don't accidentally instantiate a class instead of a nib. This is how you instantiate a class. It looks very similar, so it's an easy mistake to make.

```
collectionView.register(CreateHabitHeaderView.self, forSupplementaryViewOfKind: UICollectionView.elementKindSectionHeader, withReuseIdentifier: Text.sectionHeaderIdentifier)
```
 
If you accidentally instantiate a class, you won't be able to make any changes to the UI elements in your `.xib`, as those elements won't exist. For example, say you have a `@IBOutlet private weak var titleLabel: UILabel!`. If you try to set the title using `titleLabel?.text = "Hullo there"`, nothing will happen. This is because the `titleLabel` outlet doesn't exist. It is always `nil`, as you never instantiated the nib. Thus the outlets were never created.

Even worse, if you didn't use the optional and you tried `titleLabel.text = "Hullo there"`, your app will crash as `titleLabel` is `nil`.

### Creating a custom view to use in a storyboard

Now let's say you want to create a custom `UIView` that you can drag and drop into a storyboard. Mine is called a `TimePicker`. I need two files: `TimePicker.xib` and `TimePicker.swift`.

**1. Set up the `.swift` file**

The `TimePicker.swift` file needs to do a few things.

**Ensure it has the same class as the nib**

This allows the nib can use the `.swift` file as its custom class. 

Given our nib is a `UIView`, we need the `.swift` file to declare `TimePicker` as a `UIView` as well. ie. `final class TimePicker: UIView { }` and not `final class TimePicker: UITableViewCell { }`

**Initialise your nib**

In order for another view to use your nib, you need to initialise it correctly. Below is an example of an initialiser. Let's go through each section.

```
private func commonInit() {

        let bundle = Bundle.init(for: TimePicker.self) // 1 Initialise the bundle

        // 2 Load the nib
        if let viewsToAdd = bundle.loadNibNamed("TimePicker", owner: self, options: nil),
            let contentView = viewsToAdd.first as? UIView {
            addSubview(contentView) // 3 Add the nib to the parent view
            contentView.frame = self.bounds // 4 Set the frame
            contentView.autoresizingMask = [.flexibleHeight, .flexibleWidth] // 5
        }
}
```

**1. Initialise the bundle**
`let bundle = Bundle.init(for: TimePicker.self) // 1`

Firstly you need a `Bundle`. A bundle is initialised using a class. You have already defined the class you want at the start of the `TimePicker.swift` file: `final class TimePicker: UIView`. 

This is why you can initialise the bundle using `TimePicker.self`.


**2. Load the nib**

Next you want to load your `.xib` as part of that bundle. To do that, you use the code: `let viewsToAdd = bundle.loadNibNamed("TimePicker", owner: self, options: nil)`.

Xcode doesn't know how many different views are in the nib, `TimePicker.xib`. Xcode also doesn't know what type of views exist. The views could be `UIView`, `UITableViewCell`, `UICollectionViewCell` etc.

Thus the above line of code will return an array of objects, `[Any]`, and store this in `viewsToAdd`.

In my example, I only have one `UIView` in `TimePicker.xib`. I need to tell Xcode this information, so that I can access it later as a custom view.

To do this, I use `let contentView = viewsToAdd.first as? UIView`. Note I am casting with an optional. I know this won't fail as my nib has a `UIView`. But if the nib had something which could not be cast as a `UIView`, then `contentView` would be `nil`.

**3. Add the nib to the parent view**

Next I tell the super view to add the custom view I created in my nib, now known as `contentView`: `addSubview(contentView)`

If I skip this step, I won't see my custom view when I run the app. 

**4. Set the frame**

When I want to add this custom view to a storyboard, I first need to add a `UIView` object to the storyboard. As with all UI objects, I will need to set its constraints, ie. how this view should be positioned relative to the other objects in the storyboard, and its height and width.

These constraints will be translated into the object's "frame", which is of type `CGRect`.

So our `UIView` object on the storyboard now has its own frame, let's call it Frame S (for Storyboard).

Next, tell the storyboard to associate this view object with our custom view object, `TimePicker`. We do this by setting the custom class through the right hand nav.

Once we do this, the app will run through the initialisers in `TimePicker.swift`. 

By using `contentView.frame = self.bounds`, I am telling Xcode to make sure that the `TimePicker` view uses Frame S. This is important - it means Frame S will override the constraints that I set in `TimePicker.xib`.

For example, if Frame S has a width of 100, but `TimePicker.xib` needs a width of 400, then Frame S wins. The storyboard will truncate the width of the custom view.

If I don't want this to happen, then I can define my own frame in the `commonInit()`. This means I would write something like `contentView.frame = CGRect(0, 0, 400, 80)`.

Now we are telling Xcode to ignore Frame S when creating `TimePicker`, and to make sure that `TimePicker` always has width 400 and height 80.

There are pros and cons to either method, as always with programming. If you use Frame S, you can rely on the fact that the custom view will be at least the size of Frame S. Note that if the actual size of the nib is larger than what you specified in Frame S, the custom view will still appear in its actual size when the app runs. However its frame, which has the background color for example, will be sized as per Frame S.

If you specify the frame in the initialiser, then you know your custom view will always be the right size. However you rely on the programmer making sure that there is enough space in the storyboard to accommodate your custom view.

I haven't written this very clearly. The best way is to set a background color on the UIView and the custom view and play around with it.


*
Next I set the frame of my custom view. I have chosen to say it should follow the bounds of the `UIView` that is added to the storyboard.  I need to define the frame for this object (let's call these Frame A), and then I will set the custom class of the view object to be `TimePicker`.
*






**Override the default initialiser**

The default initialiser should do its normal thing, wich is `super.init`. However it also needs to do some things specific to your nib.

We'll put those things in a private function called `commonInit()`.
```
override init(frame: CGRect) {
        super.init(frame: frame)
        commonInit()
}
```
The final code looks like this:

```
final class TimePicker: UIView {
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        commonInit()
    }

    required init?(coder: NSCoder) {
        super.init(coder: coder)
        commonInit()
    }

    private func commonInit() {

        let bundle = Bundle.init(for: TimePicker.self)
        if let viewsToAdd = bundle.loadNibNamed("TimePicker", owner: self, options: nil),
            let contentView = viewsToAdd.first as? UIView {
            addSubview(contentView)
            contentView.frame = self.bounds
            contentView.autoresizingMask = [.flexibleHeight, .flexibleWidth]
        }
    }
}
```


**1. Set the custom class in `TimePicker.xib`**

You need to set the custom class of the File's Owner, not the view. You can see that now I have a `View` and not a `Collection View Cell`, as I dragged and dropped a `UIView` into the nib.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601430595203/2QHhDOjsT.png)

Once you select `File's Owner`, then head to the right hand nav and type in `TimePicker`. 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601431071931/G5AokjGcL.png)

You need to have already created the `TimePicker.swift` file, with at least this much code:

```
final class TimePicker: UIView {
}
```

This code tells Xcode that there is a `UIView` called `TimePicker`. This allows you to specify that your nib should have the custom class `TimePicker`. If you had `final class TimePicker: UILabel`, then you wouldn't be able to use `TimePicker` as the custom class for your nib, as the nib is expecting a `UIView`, and `TimePicker.swift` has defined `TimePicker` as a `UILabel` so there is a mismatch.

Note after you have set the custom class, you won't see any change in the above information. The `File's Owner` will still be called `File's Owner`. It won't update to the custom class name.

Also note the view itself is still called the generic `View`, as we have not set a custom class here.

**2. Use your custom view in other storyboards**

Drag and drop a `UIView` into your storyboard.
You need to set the constraints of this view in the storyboard, even though your nib already has constraints of its own.

The constraints in your storyboard will override the constraints in your nib.

For example, if your custom view has a set width of 400, but the `UIView` in your storyboard has a set width of 100, then your custom view will be truncated. It won't be squashed and re-sized into 100. Rather the remaining 300 pixels in your view will not be shown.



### Other issues I haven't solved

`Thread 1: Exception: "-[UIViewController _loadViewFromNibNamed:bundle:] loaded the \"TimePickerViewController2\" nib but the view outlet was not set."`

Thank you so much to [this excellent article](https://blog.usejournal.com/make-views-using-xib-45219ead24)  that finally made things click for me.