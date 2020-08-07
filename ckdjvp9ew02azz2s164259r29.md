## Error: Could not load NIB in bundle: 'NSBundle

If your app builds, but then crashes when you try to load a particular view, chances are you've forgotten a simple step.

This happens when you are using `.xib` files along with code. For me, I'm using a programatic table view, with `.xib`s.

This the full error message:
```
*** Terminating app due to uncaught exception 'NSInternalInconsistencyException', reason: 'Could not load NIB in bundle: 'NSBundle <long path name/AppName.app> (loaded)' with name '<Name of the nib>''
```

Some things to check:
* You have registered the nib: `tableView.register(UINib(nibName: T.dequeueIdentifier, bundle: nil), forCellReuseIdentifier: T.dequeueIdentifier)`
* The `fileName.xib` is exactly the same as the name you used to register. This is where I tripped up. I was using `nibName: ListStandardIconCell`, but my file name was `ListStandardIcon.xib`. When I changed it to `ListStandardIconCell.xib`, the crash went away.
* Have a `.swift` file that is associated to the nib, so that you can drag in the outlets

```
import UIKit

// MARK: - ListStandardIconCell

final class ListStandardIconCell: UITableViewCell {

    // MARK: Outlets
    
    @IBOutlet private weak var titleLabel: UILabel!
    @IBOutlet private weak var iconImageView: UIImageView!
}
```
* The custom class is set to the associated `.swift` for that nib (see image below)

![Screen Shot 2020-08-07 at 4.56.57 pm.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1596783439785/YmlfA7W6R.png)

