## When titleLabel.text = "Hi" crashes your app

This happens when you are using storyboards and xibs.

Here are some reasons your app could crash:
* You forgot to connect the outlet to your `.swift` file
* You are calling `titleLabel.text = "xyz"` too early. It needs to be done in `viewDidLoad`, or sometime after the view is presented. It can't be called before the view is presented.
* You are instantiating a class instead of a nib (or vice versa)

### Instantiating a class vs a nib

Let's say you have a `CreateHabitHeaderView.xib` file that you want to use inside a collection view.

You'll have a corresponding `CreateHabitHeaderView.swift` file as well, which links up to that `.xib`.

Here's what you need to do in the `.xib`.
1. Set a custom class for the overall view (circled below in red). Currently my `.xib` is a collection view cell. Yours may be a view, or a table view cell, or something else. 

![Screen Shot 2020-09-30 at 11.25.55 am.png]

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