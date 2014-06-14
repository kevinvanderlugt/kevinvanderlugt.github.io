---
layout: post
title:  "Saving bundle resources using one simple line of code"
date:   2014-06-13 10:00:00
categories: blog
---

This one simple trick has saved me quite a bit of time recently refactoring some of my old code. Now that I have all this spare time to go back and update a bunch of my apps, I am spending more time trying to reduce the images to lower the bundle size and save work making images. I was able to reduce my overall bundle size by almost 20% by just using it.

What is this magical trick you ask?  It is actually utilizing the transform on UIView to rotate an image.   Pretty simple right?  Surprisingly, I didn't use this enough in my previous apps when I was just trying to get them working and hopefully making money.  

{% highlight objective-c linenos %}
// Flip everything 180 to correlate with the correct shading
self.bottomButtonView.transform =
    CGAffineTransformRotate(self.bottomButtonView.transform, M_PI);
{% endhighlight %}

This code will simply rotate an view 180 degrees.  Previously in other apps, I was taking the same image, rotating it in illustrator, saving it as image_180.png and image_180@2x.png.  What a pain and also a waste for the users to have to download the copies of the images.  Now that I can utilize new knowledge on my old apps, I have been able to significantly reduce the images in my bundle.

Hopefully you already knew about this trick or if not, I hope you use it to save some serious resources!

Cheers!  
-Kevin

---

Things I read this week:
[IBDesignable and IBInspectable](https://iosdevweek.ly/huBxlV)
Live visualization of views and editable values in interface builder!  Very cool, I am excited to play more with this.

---

What other simple trick has really helped you save time and resources?  [Email me](mailto:kevin.vanderlugt@gmail.com) your comments and I will add them to the list