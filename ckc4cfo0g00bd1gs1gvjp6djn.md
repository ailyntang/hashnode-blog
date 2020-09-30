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

Now let's say you want to create a custom `UIView` that you can drag and drop into a storyboard. Mine is called a `TimePicker`. So I will have two files: `TimePicker.xib` and `TimePicker.swift`.

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

For example, if your custom view has a set width of 400, but the `UIView` in your storyboard 

### Other issues I haven't solved

`Thread 1: Exception: "-[UIViewController _loadViewFromNibNamed:bundle:] loaded the \"TimePickerViewController2\" nib but the view outlet was not set."`

