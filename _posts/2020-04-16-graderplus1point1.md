—--
layout: post
title: Grader+ 1.1
slug: graderplus1point1
date: 2020-04-16 16:48:34 +0100
category: linked
titlelink: https://apps.apple.com/gb/app/grader/id1448038088
tags: [Grader+, Swift, SwiftUI]
—--

I’m pleased today to announce the release of version 1.1 of [Grader+](https://apps.apple.com/gb/app/grader/id1448038088). I’ve rewritten the app from the ground up using [SwiftUI](https://developer.apple.com/xcode/swiftui/), Apple’s new declarative UI framework. This was partly so that I could learn how to use it, and partly so that the app would be easier to maintain in the future. I was _just about_ able to put the app together using UIKit for version 1.0, but it was a struggle at times. SwiftUI made that so much better, and has given me confidence about maintaining it and building more apps in the future. 

One of the key things I was aiming for in this version was simplicity. There are settings and options – and I’ve tried to put them in more accessible places – but mostly I’ve tried to make decisions which mean the app just does what you would expect it to do. It’s ultimately a very simple app for a very specific purpose, so I tried not to overcomplicate it. 

Part of that simplification process is the pricing model. The complexity of the code I had to write in version 1.0 just to handle the in-app purchase was almost beyond my abilities. In fact, I wouldn’t have been able to do it without [Ray Wenderlich’s excellent tutorial](https://www.raywenderlich.com/5456-in-app-purchase-tutorial-getting-started) on the topic. For an app as simple as this, I’ve decided it’s not worth it, so this version is just paid up front, with no in-app purchase. Most people don’t need this app, but I’m hoping the people who do will be willing to chip in. It would be nice to at least cover my developer licence! 

Thanks go to [Paul Hudson](https://twitter.com/twostraws) at [Hacking with Swift](https://www.hackingwithswift.com) for a wonderful set of resources from which to learn SwiftUI. For just about any question I had there was an answer there.

Thanks also to [Emil Baehr](https://emilbaehr.com), who put together an amazing [Jekyll template](https://github.com/emilbaehr/automatic-app-landing-page) which automatically generates an app landing page using an app’s store ID. With very little effort and only a few small edits, I was able to put together a [good-looking landing page](https://davisonreiber.com/graderplus/) for my app. 

If you’re a teacher who wants a quick and easy way to record marks while you’re grading papers or exams, try out [Grader+](https://apps.apple.com/gb/app/grader/id1448038088) for iPhone.