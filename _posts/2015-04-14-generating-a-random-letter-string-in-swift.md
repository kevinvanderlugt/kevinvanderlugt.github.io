---
layout: post
title: "Generating a random letter string in Swift"
description: "Learn how to generate a random character between A and Z in Swift. As a quick aside I thought it would be fun to compare to Ruby."
category: blog
tags: [swift]
---

I have been lately doing more Swift exercises from [Exercism.io](http://www.exercism.io/kevinvanderlugt) and thought it would be interesting to post about different common functionality.

In this post, you will learn how to generate a random character between A and Z in Swift.  As a quick aside I thought it would be fun to compare to Ruby. This was created for the RobotName exercise in [Exercism.io](http://www.exercism.io)

### Updated for Swift 4.2

Swift 4.2 and above

{% highlight swift linenos %}
let uppercaseLetters = (65...90).map {String(UnicodeScalar($0))}
func randomLetter() -> String {
    return uppercaseLetters.randomElement()!
}
{% endhighlight %}

Ruby

{% highlight ruby linenos %}
("A"..."Z").to_a.sample
{% endhighlight %}

I was surprised at how much more complex this simple method came out to in Swift.  First we have to create an array of all characters that we want to randomly select from by creating an array from a range of 65 to 90 then map this to an array of strings from UnicodeScalars.

Luckily with Swift 4.2, we now can easily get a randomElement in the array! I feel comfortable force unwrapping here because I just created the array with items in it. If you were using this with array passed to you, `if let` is a much better choice.

Before Swift 4.2
Next we select an random index from the letter array but don't forget to cast to a UInt32.  Now that we have the index, we need to cast back from a UInt32 to an Int and return the letter from the array.

In Ruby, we need to create a Range from "A" to "Z", cast that to an Array and call sample which will give us a random object from the Array. Easy!

I have been missing Ruby quite a bit while doing Exercism exercises and wonder if that is because the exercises were originally created for Ruby. Thoughts?

The full code can be found in [this gist](https://gist.github.com/kevinvanderlugt/08144bc587788d3f8e95) for easier copying.

Cheers!

-Kevin

---

Do you have a better way to do this in Swift? [Email me](mailto:kevin.vanderlugt@gmail.com) your comments and lets chat!


