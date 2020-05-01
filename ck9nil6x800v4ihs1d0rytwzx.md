## Inject view model in a view

There are two major patterns I have come across.

* Pattern 1: `init(with viewModel: ViewModel)`
* Pattern 2: `init()` and `render(using viewModel: ViewModel)`

Pattern 1 is best with your view should only be rendered with one view model.

Pattern 2 is best if your view needs to be updated with a new view model (e.g. based on an API call).
