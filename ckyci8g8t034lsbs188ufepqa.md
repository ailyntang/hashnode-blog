## Navigation bar UI tips

### A view that goes over the top of the nav bar
```
guard let navigationController = navigationController else { return }
let overlay = UIView()
navigationController.view.addSubview(overlay)
overlay.topAnchor.constraint(equalTo: navigationController).isActive = true
// set up the rest of the autolayout constraints
```

### Useful heights
```
let navigationBarHeight: CGFloat = navBarHidden ? .zero : navigationController?.navigationBar.frameHeight ?? .zero
        let statusBarHeight: CGFloat = UIApplication.shared.keyWindow?.safeAreaInsets.top ?? .zero
```