## Background threads

The theory is you do all your API calls and stuff that takes ages on background threads.
Then when it's ready, you come back and update your UI on the main thread.
(UI must always be updated on the main thread).

Reason: if you don't do things on the background thread, then your UI will be frozen whilst stuff completes.

For API calls, often times it's not a big deal, because there's nothing else your user can do until that mutating API call finishes anyway. So you stick a loading state UI up there to indicate what is happening, and then the API call could be on the main thread.

But if you are doing something like updating a database, this could take ages.
And if you need to update the database PLUS update UI, and both are on the main thread, then you have no control over which happens first.

So you would put the database work on a background thread, as it doesn't affect the UI.