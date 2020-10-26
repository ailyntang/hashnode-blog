## MVVM with storyboards: now easily done with iOS 13

Code inside your view controller:

```
init?(coder: NSCoder, viewModel: SleepViewModel) {
        self.viewModel = viewModel
        super.init(coder: coder)
    }
    
    required init?(coder: NSCoder) {
        fatalError("Programmer error: SleepViewController initialised without a view model")
    }
```

Code inside the presenting view controller:

```
extension HomeViewController: UITableViewDelegate {
    
    func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
        
        tableView.deselectRow(at: indexPath, animated: true)
        
        let storyboardID = viewModel.cellViewModels[indexPath.row].storyboardID
        let storyboard = UIStoryboard(name: storyboardID, bundle: nil)
        let viewController = storyboard.instantiateViewController(identifier: storyboardID) { coder in
            return SleepViewController(coder: coder, viewModel: SleepViewModel())
        }
        show(viewController, sender: self)
    }
}

```

# Prior to iOS 13

Prior to iOS 13, it was really tough to use MVVM with storyboards. So if you wanted a storyboard generated view controller to have a view model, there was quite a long process to go through.

This [Medium article](https://medium.com/wolox/viewmodel-injection-in-viewcontrollers-with-storyboards-50230143c3ac) is great and explains how to work around it. The required "fix" is quite detailed.

A different workaround to use MVVM more simply, prior to iOS 13, was to forego the view controller storyboard. Use `.xib` files instead. This way I can programatically initialise the view controller and inject a view model simply, rather than needing the detailed fix outlined in the Medium article.
