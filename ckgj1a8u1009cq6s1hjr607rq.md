## App crashes when trying to underline text in a button

This is the error: ` *** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '-[__SwiftValue _getValue:forType:]: unrecognized selector sent to instance 0x6000008927c0'`

This is the code that causes the error:

```
func styleButton(_ button: UIButton) {

        let attributes: [NSAttributedString.Key : Any] = [
            NSAttributedString.Key.underlineStyle: NSUnderlineStyle.thick,
            NSAttributedString.Key.underlineColor: UIColor.blue
        ]
        
        let attributedString = NSAttributedString(string: "yoyo", attributes: attributes)
        button.setAttributedTitle(attributedString, for: .normal)
}
```

The `unrecognized selector` is `NSUnderlineStyle.thick`.

The key is expecting a value which is an integer. So I need to replace this value with `NSUnderlineStyle.thick.rawValue`.

It's strange to me that the app compiles with this error, and doesn't really give a better hint to solve it.