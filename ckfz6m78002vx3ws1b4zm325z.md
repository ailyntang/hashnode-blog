## Present a modal which is less than full screen

Seems like there are a few ways to do this. None are that straight forward.

1. Create a `UIViewController` and set the background of the top section as clear

2. Use a `UIPresentationController`

#1 seems like a hack. Smoke and mirrors :) 

#2 seems like the proper way to do it. But if your base `UIViewController` is larger than the expected size, things won't look very nice in the UI.

Here's the code for #2. Here's how my app works:

* `SleepViewController` is the base VC. When you tap a button on this VC, it will present a modal created from `TimePickerViewController`

* `TimePickerViewController` is the modal that will be presented

```
final class SleepViewController: UIViewController {

   @IBAction func tapStartTimeButton(_ sender: UIButton) {
        
        let viewController = TimePickerViewController()
        viewController.modalPresentationStyle = .custom // must be .custom to use the presentation controller
        viewController.transitioningDelegate = self // must set the delegate to use the presentation controller
        present(viewController, animated: true, completion: nil)
    }
}

extension SleepViewController: UIViewControllerTransitioningDelegate {
    
    func presentationController(forPresented presented: UIViewController, presenting: UIViewController?, source: UIViewController) -> UIPresentationController? {
        
        return HalfSizePresentationController(presentedViewController: presented, presenting: presenting)
    }
}

final class HalfSizePresentationController: UIPresentationController {
    
    override var frameOfPresentedViewInContainerView: CGRect {
         return CGRect(x: 100, y: 300, width: 200, height: 300)
    }
}

```