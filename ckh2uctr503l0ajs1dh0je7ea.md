## UI element doesn't appear

Even when it shows up in the visual debugger as a line item, but it's not drawn.

It's because it is missing constraints. As Xcode doesn't know how to place it, it just ignores it.

For some reason, this wasn't called out to me as a warning in the xib. Shrug.