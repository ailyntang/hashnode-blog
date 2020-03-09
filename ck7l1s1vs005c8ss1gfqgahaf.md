## Label not wrapping on trailing edge

If you are using constraints, don't forget the for the trailing edge constraint you need to use a negative constant.

So it will look something like:
```swift
descriptionLabel.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 20).isActive = true
descriptionLabel.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -20).isActive = true
```
