## Tableview cells, prepareForReuse

# `UITableViewCell`

Once you use a cell in a table view (or really any UI element like an annotation in MapKit), the compiler(? app?) will hang on to that UI element to see if it can re-use it again.

If you then need the same type of that UI element, it will check to see if it already created one. If so, great, it will be re-used.

If not, then it will make a new one.

Example:
I have a table view cell: `LargeImageWithText: UITableViewCell`.

When I first load my screen, there are 2 cells of this type in the tableview.

The common way cells are re-used in tableviews are through scrolling. So if you a list of 200 banking transactions, and you only display 10 at a time, then the app will only ever make 10 cells. And it will keep updating those cells to display the next 10 banking transactions as you scroll.

Another way is if the UI changes. 
Let's say you start with only 1 banking transaction in the table view.
Then you tap a button that says: Show me 10 transactions.

The table view will then look in its library to see if it has 10 of those cells. It only has one. It will re-use that one cell, and then create 9 more.

It can re-use that one cell even in a different spot. So the creation of the cell is the important part. The cell can be at the top of the screen, then a whole bunch of images, then the next time the table view UI changes and the images are at the top and the cell at the bottom. That's fine - the tableview can still reuse the cell.

What's important is that you implement the `prepareForReuse` function.
If there is a way for the cell to be rendered with optional things, and the rendering function doesn't clear out those optional things, then you'll see weird behaviour when scrolling (inconsistently weird behaviour).

Example: I had an image that was a company logo, if the logo was available, or a letter if it wasn't. I didn't remember to clear the text and the logo when preparing for reuse, nor in the rendering. So logos and letters were stacking up on top of each other.

# `UIDatePicker`
Here I am using an inbuilt delegate to tell me when the user selects a new date:
 `datePicker.addTarget(self, action: #selector(handleDateSelection), for: .valueChanged)`

To clear this, I should use `removeTarget`:
`datePicker.removeTarget(self, action: #selector(handleDateSelection), for: .valueChanged)`