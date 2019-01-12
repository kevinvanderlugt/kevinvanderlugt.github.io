---
layout: post
title: "Pro Swift by Paul Hudson - Book Review"
description: "Pro Swift is totally worth the cost. It is a great resource for people already writing Swift in apps and want to learn a few new things."
category: "books"
tags: [swift]
published: true
---

## Why did I want to read this book?

I started iOS development (self taught) during college.

I was super excited about building apps, wanted to get started and began with Objective-C and shipping projects.

It turns out that approach worked out really well, I shipped stuff!

Made a little bit of money on the side and worked into a career in iOS development.

Then Swift came along, it was great!

I learned through Apple's Swift book and shipping even more projects.

I eventually worked up to building safe Swift code in the Rover app.  Nowadays, we write everything new in Swift!

However, being self taught, there was always a feeling that I was missing out on some really great features of the language.

## Enter "Pro Swift" by Paul Hudson

I have read a few of [Paul's books](https://www.hackingwithswift.com/store) and his pragmatic approach to building apps is super refreshing. I grabbed Pro Swift recently just to fill in any gaps that I might have missed by being self taught and could bring to my team at Rover.

Pro Swift is an excellent resource for anyone that has a bit of experience with Swift but wants to learn a few new tricks.

Overall, I would say about 90% of the stuff I already knew but picked up a few new pieces of knowledge that I can use in my work.

### Pro Swift can get you writing better Swift quickly

For it's price, you are almost certain to pick up some new tricks and tools which will certainly pay for itself through your careers.

## What did I learn?

I thought it would be helpful to list a few of the things I learned.
This definitely doesn't cover the whole book and you should get it yourself.

That said here are a few of my new favorites. If you know all of these, I am still certain you will learn at least one new thing.

### Default values in Swift dictionaries

I actually had learned this but it slipped my mind at some point.

Using a default value for a Swift dictionary can be a great way for counting things.

Normally, if you were iterating over a list and wanted to add up the times it occurred you would need to check if a value for the key exists, if so, increment it and then store it back in the dictionary, if it didn't exist you would need to just set it to 1.

```
var favoriteTVShows = ["Red Dwarf", "Blackadder", "Fawlty Towers", "Red Dwarf"]
var favoriteCounts = [String: Int]()
for show in favoriteTVShows {
    var count = favoriteCounts[show] ?? 0
    count += 1
    favoriteCounts[show] = count
}
```

A Swift default dictionary makes this code much simpler. The will get set to 0 if the key does not exist yet then you can just increment. Much cleaner!

```
let favoriteTVShows = ["Red Dwarf", "Blackadder", "Fawlty Towers", "Red Dwarf"]
var favoriteCounts = [String: Int]()
for show in favoriteTVShows {
  favoriteCounts[show, default: 0] += 1
}
```

### @autoclosure and using them with asserts

Paul goes into great depth on `@autoclosure`, it's one of those things that is really good to understand but I just haven't really needed to use.

However, right near the end of the chapter he really drops the 💣.

"The most common use for this is when using a logging system: your message closure can write an error to your log, before returning that message back to assert()."

This use case in asserts is a great idea. This was probably the best thing I learned through the book.

`assert(1 == 2, saveError(message: "Fatal error!"))`

### allSatisfy

This one I debated talking about because I am a little embarrassed I haven't used it more 🙈.

I have used `first(where:)` on `Array` to find the first matching item and then breaking out early.

Swift’s allSatisfy() method is a similar improvement if you need to check that every element in an array matches a condition. It will check over the full array and return false as soon as any element fails the predicate:

## Go get it today

[Pro Swift](https://www.hackingwithswift.com/store/pro-swift) is definitely worth the cost!

You will almost certainly learn something that you didn't know before.

It's easy to pick up and jump around different chapters. You don't have to read it sequentially but can if you want.

Seriously... [go get yourself a copy](https://www.hackingwithswift.com/store/pro-swift).

## Did you read Pro Swift?

I would love to hear from others on their thoughts on this book. Or if you have any thoughts on what book I should read next?
[Leave me a comment on this Github issue](https://github.com/kevinvanderlugt/kevinvanderlugt.github.io/issues/10).