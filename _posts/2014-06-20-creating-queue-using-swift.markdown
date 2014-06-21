---
layout: post
title:  "How to create a queue in swift"
date:   2014-06-20 10:00:00
categories: blog
---

This week I decided to spend some time creating a Queue using an Array in Swift.  I am working through the excellent documentation from Apple and thought I would give it a shot.  Here is my first attempt at writing the queue in swift playground. 

A queue can be a great structure to use for your apps which I regularly use to batch my URL requests.  I often use the queue in my application to store a list of objects downloaded from remote APIs. When the queue gets too low from the user requesting to see these objects, I can create a NSURLSession to add more objects to the queue.  By playing with the queue limits is a great way to do fetching without your users ever knowing that the remote data is being loaded.

Feel free to use this code anywhere you want, if you have any suggestions let me know!  Also note, syntax highlighting wasn't available at the time of writing :(

{% highlight objective-c linenos %}
class Queue {
    var items: Array<AnyObject> = []
    
    // Add an object to the tail of the queue
    func enqueue(object: AnyObject) {
        items += object;
    }
    
    // Remove the head of the queue and return the object
    func dequeue() -> AnyObject? {
        assert(items.count > 0, "Array has no items to dequeue")
        let object : AnyObject? = items[0]
        items.removeAtIndex(0)
        return object
    }
    
    // Empty the entire queue
    func clear() {
        items.removeAll(keepCapacity: false)
    }
    
    // Return if the queue is empty
    func empty() -> Bool {
        return items.count == 0
    }
    
    // Implement the subscript to peek at any index using queue[1] notation
    subscript(index: Int) -> AnyObject? {
        assert(index < items.count, "Index is out of bounds")
        return items[index];
    }
    
    // Return the object at the index a wrapper for using subscript notation
    func peek(index: Int) -> AnyObject {
        return items[index]
    }
    
    // Return the object at the front of the queue without mutation
    func peekHead() -> AnyObject {
        return items[0]
    }
    
    // Return the object at the end of the queue without mutation
    func peekTail() -> AnyObject {
        let index = items.endIndex - 1
        return items[index]
    }
}
{% endhighlight %}

Here are a few ways that I was playing around with the Queue. 
{% highlight objective-c linenos %}
var testQueue = Queue()
testQueue.enqueue("Kevin")
testQueue.enqueue(1)
testQueue.enqueue(1.000)

let peek = testQueue.peek(2)
let peekTail = testQueue.peekTail()
let subs = testQueue[2]

if(peek === subs) {
    println("Same Object")
}
{% endhighlight %}

Overall, I think swift has been much clearer in its method definitions.

Cheers!  
-Kevin

---

Did I miss a method that you think would be useful?  Do you have any ideas on how to make this better? [Email me](mailto:kevin.vanderlugt@gmail.com) your comments and I will add them to the list