## Unexpectedly found nil when unwrapping an optional

This happens if you are using a storyboard / nib / xib file, and you accidentally register a nib when you meant to register a non-nib file.

Take a look:
```
register(T.self, forCellReuseIdentifier: T.dequeueIdentifier)
register(UINib(nibName: T.dequeueIdentifier, bundle: nil), forCellReuseIdentifier: T.dequeueIdentifier)
```

Those are two different functions - make sure you choose the correct one