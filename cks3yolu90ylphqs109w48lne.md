## Closures, completion handlers, delegates

They do the same thing.
Closures are completion handlers. Same same.

But, it's good to use closures when there's one path. E.g. On success, on completion.

When there are multiple possible paths, then delegates are clearer.
E.g. if x, do y. If a, do b. And sometimes the path is x, sometimes it's a.