## Can't click on any UI elements

If you find yourself unable to click on any UI elements, check that you have set all your constraints.

If a constraint is missing, then your view may present itself, but you may not be able to click on any element.

For example, I forgot to set the height anchor in the code below, and thus was not able to use the date picker or tap any of the buttons.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602630593906/aqKq-w_Da.png)

```
@IBAction func tapStartTimeButton(_ sender: UIButton) {
        
        view.backgroundColor = UIColor.white.withAlphaComponent(0.8)
        
        let timePickerViewController = UIViewController()
        timePickerViewController.view.addSubview(timePicker)
        timePickerViewController.view.backgroundColor = .white
        
        timePicker.delegate = self
        timePicker.updateTitle(to: "Start Time yoyo")
        timePicker.translatesAutoresizingMaskIntoConstraints = false
        timePicker.topAnchor.constraint(equalTo: timePickerViewController.view.topAnchor).isActive = true
        timePicker.leadingAnchor.constraint(equalTo: timePickerViewController.view.leadingAnchor).isActive = true
        timePicker.trailingAnchor.constraint(equalTo: timePickerViewController.view.trailingAnchor).isActive = true
        // Missing the height anchor
        
        timePickerViewController.modalPresentationStyle = .custom
        timePickerViewController.transitioningDelegate = self
        self.present(timePickerViewController, animated: true, completion: nil)
    }
```