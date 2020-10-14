## This class is not key value coding-compliant for the key <Start Button>

This means you have not connected a UI element in your storyboard or nib.

If you're lucky, when you head to the outlet section on the right hand nav, there will be a warning, like below.

In this instance, there's a missing outlet connection. It's missing because I connected the outlet but then deleted the outlet code in the `.swift` file.

I also need to delete this outlet connection which has a warning in the nib. I can do this by clicking the little cross next to the "Start Button".

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1602673445592/vbIWvHlwb.png)
