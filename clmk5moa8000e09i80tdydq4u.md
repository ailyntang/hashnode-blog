---
title: "SwiftUI: state management across UI elements"
datePublished: Fri Sep 15 2023 05:23:56 GMT+0000 (Coordinated Universal Time)
cuid: clmk5moa8000e09i80tdydq4u
slug: swiftui-state-management-across-ui-elements
tags: swiftui

---

[https://developer.apple.com/tutorials/app-dev-training/managing-data-flow-between-views](https://developer.apple.com/tutorials/app-dev-training/managing-data-flow-between-views)

# `@State` or `@Published` : source of truth

* Value types (`struct` or `enum`): use `@State private var`
    
    * `@State` is a property wrapper
        
* Reference types (`class`): use `@Published var`
    
    * `@Published` is an attribute
        
    * `@Published` is not private, because other views will need access to this property
        
    * Ensure the class is observable, by conforming to the `ObservableObject` protocol, e.g. `final class EditWorkoutView: View, ObservableObject`
        
    * When instantiating the class, make the entire class observable by using one of these property wrappers: `@ObservableObject`, `@StateObject`, `@EnvironmentObject`
        
        * e.g. `@ObservableObject private var editWorkout = EditWorkoutView()`
            
* The `@State` or `@Published` variable is the piece of information that views need to update
    

It is a transient state. Examples:

* `isPresentingEditView: Bool`: when true, the view presents a modal; when false, the view dismisses the modal
    
* `selectedTheme: Theme` where `Theme` is a color used in the app. When the user updates the selected theme, then we want all views which reference this theme to update to the newly selected theme.
    
* `workouts: [Workout]` the full list of possible workouts. When a user edits an individual workout, this is a binding which is then passed back to the original source of truth, `@State private var workouts: [Workout]`
    

# Working with state management and classes

[https://developer.apple.com/tutorials/app-dev-training/making-classes-observable](https://developer.apple.com/tutorials/app-dev-training/making-classes-observable)

To observe properties inside a class:

* `final class EditWorkoutView: View, ObservableObject`
    
* Then use `@Published` properties inside the class: `@Published var oneRM: Double = 50.0`
    

To observe the class itself:

* `@StateObject var editWorkout = EditWorkoutView()`
    
    * This is like `@State`. It's the source of truth
        
* `@ObservableObject var editWorkout: EditWorkoutView`
    
    * This is like `@Binding`. You instantiate a view with the `@StateObject`
        
    * So you pass the `@StateObject` around using `@ObservableObject`
        
    * This gets a bit tedious at times, if you have lots of nested views. You have to inject every layer of view with the `@ObservableObject`, just so one view all the way at the end can use it.
        
    * What's the alternative? `@EnvironmentObject`
        
* `@EnvironmentObject` feels a bit like a lazy `singleton` hack
    
    * I don't know if there are any drawbacks to `@EnvironmentObject` yet. Maybe it's totally fine!
        
    * You modify a view using `.environmentObject(editWorkout)`
        
    * Then every nested view can access the `@EnvironmentObject` without needing it to be passed through the initialiser.
        

```plaintext
final class PercentageBreakdownCalculator: ObservableObject {}

struct ParentView: View {
   @StateObject var calculator: PercentageBreakdownCalculator = .init()
   var body: some View {
      ChildView()
         .environmentObject(calculator)
   }
}
```

```plaintext
// Note the ChildView has zero reference to the calculator, because it doesn't need it
// So the use of @EnvironmentObject keeps the code clean in ChildView
struct ChildView: View {
   var body: some View {
      GrandchildView()
   }
}

struct GrandchildView: View {
   @EnvironmentObject var calculator: PercentageBreakdownCalculator

   // now calculator doesn't need to passed in; nor does it need to be set
   // GrandchildView can magically just use the calculator
}
```

# `@Binding`: connects you to the source of truth in a different view

Notes

* Use this property wrapper when you want to update a view which is in a different view hierarchy to the `@State` property
    
* `@Binding` connects to the `@State` property, so that the `@State` property remains the source of truth
    
    * This connection happens via the initialiser, see code below
        
* Now you can also update a different screen based on the value stored in the `@State` property
    
* If you want to mock a binding, and you don't have a `@State` property to use, then you can use `.constant(<value>)`
    
    * e.g. `WorkoutPicker(selectedWorkout: .constant(.deadlift))`
        

`WorkoutPicker` has a binding to the selected workout.

However the source of truth for the selected workout is held elsewhere in the app, in a different view hierarchy, by a `@State` property.

```plaintext
import SwiftUI

struct WorkoutPicker: View {
   @Binding var selectedWorkout: Workout

   var body: some View {
       Picker("Workout", selection: $selectedWorkout) {
           ForEach(Workout.allCases) {
               WorkoutView() // this is some view
           }
       }
   }
}
```

The `WorkoutOfTheDayDetailsView` holds the source of truth for the selected workout.

Also, this view pushes to the `WorkoutPicker`

So you pass in the `@State` property to the `WorkoutPicker` binding:

`WorkoutPicker(selectedWorkout: $workout)`

```plaintext
import SwiftUI

struct WorkoutOfTheDayDetailsView: View {
   @State private var workout: Workout

   var body: some View {
       // Views to edit the workout, e.g. enter new one RM
       // One of the views is to select the workout done today
       WorkoutPicker(selectedWorkout: $workout)
   }
}
```

Now, remember that bindings can read and write to the `@State` property.

So as the user updates the workout picker with a new workout, the new workout value flows back to the `@State` property

# TextField: always has a binding to a value

When there's a text field, you are asking users to input some information.

e.g. Type in their name, address, phone number, etc.

In SwiftUI, the data entered by the user is "bound" to a @State property.

* As the user enters data in real time, that value of that property is updated in real time. Without the dev needing to explicitly write code to save this property.
    
* This "binding" is visually shown with the `$`
    

```plaintext
import SwiftUI

struct EnterUserDetails: View {
   @State private var userName: String = ""

   var body: some View {
      TextField("Username", textValue: $userName)
   }
}
```

If you weren't using SwiftUI, then maybe you'd need to explicitly intercept the text field using the `UITextFieldDelegate` action to figure out the user's input.

Maybe?

Surely `UIKit` is smart enough to do this for us. Anyhoo. Perhaps that's the difference.

```plaintext
import UIKit

struct EnterUserDetails: UIView {
   private let textField = UITextField()
   private var userName: String = ""
}

extension EnterUserDetails: UITextFieldDelegate {
   func textFieldDidEndEditing(_ textField: UITextField) {
       // hmmm maybe you need to update it here?
       // Though.. surely UIKit is smart enough to do this for you as well
       userName = textField.input // or whatever the real code is
   }
}
```

### Doodles

Single source of truth: `selectedSchedule: ScheduleType`

* This is a `@State` property wrapper
    
* The `@` denotes a property wrapper, which allows for a quick way to initialise something (ie it hides away all the boilerplate code you would otherwise need to write to get a `State` variable)
    
* `@State` properties manage transient states, e.g. a selected list item. Thus:
    
    * These properties should be `private` and used locally in one view
        
    * Do not use `@State` properties for persistent storage.
        

Observers who update their UI based on this truth:

* Modal UI element: schedule type 1 and schedule type 2