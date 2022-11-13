# UILabel: pin text to the top of the label

In a stackview, I wanted the text to pin to the TOP of the ui label.
But, it wouldn't.

Instead, the label kept expanding to bigger than the text, and then the text was vertically centered.

A quick search on SO suggested that what I wanted to do was not actually possible.

I realised what was happening was this:
* My label was in a vertical stackview
* The stackview had a forced height, which was larger than its elements
* So the stackview was choosing an element to make larger, and it chose my label


In reality, we should never need to pin the text to the top of the label.
The label should always be the size of its text, not bigger.
Which is why we can change the horizontal text alignment, but not the vertical alignment.

So, it was my stack view that was causing this issue.
And to solve this issue, I needed to manipulate the stackview to STOP making my label bigger than its text.

This is what I did to solve the issue:
* Add a new element to the stackview, a dummy view. This view will be the one that expands in height, if need be
* Set the compression hugging resistance to high for my label, to tell the stackview - don't expand me!

```

    private lazy var verticalStackView: UIStackView = {
        let stackView = UIStackView()
        stackView.axis = .vertical
        stackView.spacing = .small
        stackView.alignment = .center
        stackView.distribution = .fill
        stackView.clipsToBounds = false
        return stackView
}

func setupUI() {
        let dummyView = UIView().forAutolayout()
        verticalStackView.addArrangedSubview(headingLabel)
        verticalStackView.addArrangedSubview(subHeadingLabel)
        verticalStackView.addArrangedSubview(dummyView)

        headingLabel.setContentHuggingPriority(.defaultHigh, for: .vertical)
        subHeadingLabel.setContentHuggingPriority(.defaultHigh, for: .vertical)
}
```