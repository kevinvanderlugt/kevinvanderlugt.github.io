---
layout: post
title:  "Animating the hidden property on UIView"
description: "A simple trick to allow you to animate the hidden property on UIView by using Objective-C categories"
date:   2014-06-05 10:00:00
categories: blog
---

What a week it has been with all of the new WWDC content.  I was blown away at all the new features and have spent most of week watching videos and learning new topics so this will be a short one.

Recently, I was working on a project where I wanted to animate hiding a UIView.  The goal was to have a UIView that would animated and fade out, I was hoping that the .hidden BOOL would work for me.  Sadly, this property isn't able to be animated as I expected so I ended up using categories to animate the .alpha value.  Here is how you can do the same to achieve a slow fade out on any UIView.

We are using objective-c categories to create new methods on UIView, when doing this you should always prefix your methods with your class prefix, this prevents collisions in case Apple ever decides to implement the same method name.  For more information out the great documentation from [Apple](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html)

{% highlight objective-c linenos %}
@interface UIView (ACAAnimateHidden)

-(void)ACA_setHidden:(BOOL)hidden animate:(BOOL)animated;
-(void)ACA_setHidden:(BOOL)hidden animate:(BOOL)animated duration:(float)duration;

@end
{% endhighlight %}

All we need to do is animate the alpha value from 1 to 0 to hide or 0 to 1 to show the UIView again.  This will only work for projects greater than iOS4 but you probably should be targeting iOS6 and greater at this point.

{% highlight objective-c linenos %}
#import "UIView+ACAAnimateHidden.h"

@implementation UIView (ACAAnimateHidden)

-(void)ACA_setHidden:(BOOL)hidden animate:(BOOL)animated
{
    [self ACA_setHidden:hidden animate:animated duration:0.5];
}

-(void)ACA_setHidden:(BOOL)hidden animate:(BOOL)animated duration:(float)duration
{
    if(!animated) {
        [self setHidden:hidden];
        return;
    }

    if(self.hidden)
    {
        self.alpha = 0;
    }

    [UIView animateWithDuration:duration
                          delay:0.0
                        options:UIViewAnimationCurveEaseInOut
                     animations:^{
                         if(hidden)
                             self.alpha=0;
                         else {
                             self.hidden=NO;
                             self.alpha=1;
                         }
                     }
                     completion:^(BOOL b) {
                         if(hidden)
                             self.hidden=YES;
                     }];
}

@end
{% endhighlight %}

Now we have a simple way to animate hiding our UIViews!
{% highlight objective-c linenos %}
[self.factsBackground ACA_setHidden:NO animate:YES];
{% endhighlight %}

One bonus might be to capture the alpha value during hiding an restore it during showing again.  My current method only sets the alpha to 1.0 always because I usually try to follow [YAGNI](http://www.google.co.th/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CCMQFjAA&url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FYou_aren't_gonna_need_it&ei=yNWOU5nqOore8AX_6YKADA&usg=AFQjCNENm4W76F7KgUIspFJfQFkPKeojTA&sig2=MknvDW-sIT_yDToRY23whA&bvm=bv.68235269,d.dGc).  Add the functionality when you need it.


Cheers!
-Kevin

---

Things I read this week:

Most of my week has been following WWDC, I am particularly excited about [HealthKit](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS8.html#//apple_ref/doc/uid/TP40014205-SW19) and [Widgets](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/ExtensibilityPG/NotificationCenter.html).  I think this will open a huge amount of opportunity for new applications.  Nothing much to share but if you haven't (who hasn't?) you should definitely checkout at least the [WWDC Keynote](http://www.apple.com/apple-events/june-2014/) for all the great new features.

---

What are you most excited about from the new WWDC announcements?  [Email me](mailto:kevin.vanderlugt@gmail.com) your comments and lets chat!