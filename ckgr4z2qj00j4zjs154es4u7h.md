## Create string of the class name

Use `String(describing: Self.self)`, which will give you a string such as `"SleepViewController".
This is the same as using `String(describing: type(of: self))`.

However you do not want to use `String(describing: self)`. This gives a much more complex string such as `"<__lldb_expr_160.SleepViewController: 0x7fec8bf09930>"`


Useful for things like: ` bundle.loadNibNamed(String(describing: Self.self), owner: self, options: nil)`

It means if you want to refactor your class name, you don't need to go around changing `loadNibNamed("TimePicker")` to `loadNibNamed("DateTimePicker")`, as `String(describing Self.self)` will do it for you.

