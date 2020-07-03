## Setting UIButton titles

https://stackoverflow.com/questions/26326296/changing-text-of-uibutton-programmatically-swift


> 
Just a clarification for those new to Swift and iOS programming. Below line of code:
button.setTitle("myTitle", forState: UIControlState.Normal)
only applies to IBOutlets, not IBActions.
So, if your app is using a button as a function to execute some code, say playing music, and you want to change the title from Play to Pause based on a toggle variable, you need to also create an IBOutlet for that button.
If you try to use button.setTitle against an IBAction you will get an error. Its obvious once you know it, but for the noobs (we all were) this is a helpful tip.
