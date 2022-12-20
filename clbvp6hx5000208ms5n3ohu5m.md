# Nested stack views and ambiguous layout warnings

The visual debugger showed a purple warning for my nested stackviews.

I had one stackview, which was a progress bar with a left and right section.

This was fine, no warnings here.

But, I combined four of those views into another parent horizontal stackview.

And the parent stackview was unhappy, saying that each child stackview had an ambiguous width, and the last three also had ambiguous horizontal placements.

The fix is to use `parentStackview.distribution = .fillProportional` instead of `.fill`