## Can't tap on textfield: need to use contentView

Text field looks fine, it is rendered correctly, but you can't tap on it.

When this happened to me, it's because I was adding the text field to the view, instead of the `contentView`.

```
final class CustomCell: UITableViewCell {

    override init(style: UITableViewCell.CellStyle, reuseIdentifier: String?) {
        super.init(style: style, reuseIdentifier: reuseIdentifier)

        // Incorrect way - won't be able to click on text field
        addSubview(textField)

        // Correct way
        contentView.addSubview(textField)
    }
}
```
Side note: never directly add to the table view cell view. 
Always add views to the `contentView`.

Apple uses the content view to manage all sorts of things, like sliding, taps etc. 
The fact that developers can still accidentally add things to the cell rather than the content view seems like a legacy issue that Apple is no longer supporting.

The tricky part is that if you add it to the view by accident, things look normal. But you should never add to the cell view as there could be unintended or unknown side effects.