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