## Remove arranged subviews from stackview

For future reference to remove the arranged subviews completely you need to call

```
stack.arrangedSubviews.forEach {
      stack.removeArrangedSubview($0)
      $0.removeFromSuperview()
}
```