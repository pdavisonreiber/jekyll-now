---
layout: post
title: Grader+
slug: graderplus
category: 
titlelink: 
tags: [Grader+, Swift]
---

Today I’m very excited to announce the release of my first ever iOS app, Grader+. It’s a simple utility app, primarily designed for teachers marking tests and exams. It’s available on the App Store now as a free download, with a $0.99 in-app purchase to unlock the premium features.

My motivation for creating it was twofold. Firstly, it’s an app that I wanted to exist so I could use it myself. The way I mark, I go through tests question by question, writing the number of marks for each question on the paper. At the end, I then need to add up the marks for each student to work out their total mark and then record it. Grader+ is designed to help with the adding up and recording part of that. Now you might think – as a maths teacher – that I would be good at adding up, but in practice I would frequently lose count and have to start again, especially when I had to stop and re-mark something I had missed along the way. Other options include using a calculator, and just pressing the plus button every time, but that doesn’t solve the recording problem; or recording individual marks directly into a spreadsheet, a technique that can be useful if you want to analyse the breakdown of marks question by question, but is also time-consuming and difficult to do on the go. What I wanted was a way to record an individual mark with a single tap, and then save each mark quickly and easily. That core functionality was the inspiration of this app, and then the other features grew from there.

Secondly, my motivation for wanting to build an iOS app from scratch was to begin to properly learn the Swift programming language and the ways it can be used to build apps. I also wanted to get to grips with Xcode, the Mac app one uses to actually put the parts of the app together. I learnt a lot of the fundamentals of iOS app design from this process, especially UI design – a part of programming that I had never done before. I’ll talk more below about some of the resources I’ve found most helpful.

## Grader+ Features

Firstly, I want to run through the basic features of the app and how it’s intended to be used, and I think it makes sense to do this screen by screen.

### Counter
The counter tab of the app is where marks can be entered. By default it counts up, but this can be changed in settings if you are used to beginning from a maximum score and taking marks off. Alongside the total so far, the counter display also shows the last few marks entered (to help you keep track) as well as percentages[^1], grades[^2], and ranking. Saving a score will prompt for a student name, which is then saved, along with the score, to the scores tab.

### Scores
The scores tab is where scores are recorded, and can be configured in settings to display percentages and grades and to sort the results in various ways. There is also an option to display statistics for all of the scores. The first of the premium features is available on this screen in the share button, which allows you to export the scores to a CSV file for easy importing to a spreadsheet, which is where these things usually end up.

### Grades
This screen allows you to create and edit grade boundaries. They can be be entered as percentages or as marks[^3]. The percentage for each one is the minimum threshold at which a grade should be awarded.

### Settings
I’ve mentioned most of the settings already, but one notable one I haven’t is the other premium feature: dark mode! Why? Because it looks cool.

## Learning Swift, iOS, and Xcode

There are a lot of great resources out there for learning how to make iOS apps, but I wanted to mention a few of the ones I found most useful. As a total beginner, I wanted to _see_ someone actually make an app from scratch in Xcode to see what the process involved, and I discovered a course from Stanford University called [Developing iOS 11 Apps with Swift][4], available on [iTunes U][5], as a [podcast feed][6], and also on [YouTube][7]. I watched the entire series, completing some of the assignments as I went along, and the lecturer, Paul Hegarty, does a really good job of explaining the ideas and techniques. The course does assume knowledge of the concepts of object-oriented programming, so go and do some reading on that first if you’re not familiar.

When I got stuck, I would google my issue, and I frequently found the solutions on one of the following websites:
- [https://www.raywenderlich.com][8]
- [https://www.hackingwithswift.com][9]
- [https://www.swiftbysundell.com][10]
- And of course, [https://stackoverflow.com][11]

In particular, I would not have been able to have figured out in-app purchases without [this great tutorial][12].

---

I hope you enjoy using the app. It’s available now on the App Store, and if you want to support its development, and the development of future apps, please do unlock the premium features and throw a dollar (or a pound) my way.


[^1]: Requires a maximum score to be entered in Settings
[^2]: Requires grade boundaries to be entered in the Grades tab
[^3]: The latter only if the app knows the maximum score
[4]: https://web.stanford.edu/class/cs193p/cgi-bin/drupal/blog/15
[5]: https://itunes.apple.com/us/course/developing-ios-11-apps-with-swift/id1309275316
[6]: https://itunes.apple.com/us/podcast/developing-ios-11-apps-with-swift/id1315130780?mt=2
[7]: https://www.youtube.com/playlist?list=PLPA-ayBrweUzGFmkT_W65z64MoGnKRZMq
[8]: https://www.raywenderlich.com
[9]: https://www.hackingwithswift.com
[10]: https://www.swiftbysundell.com
[11]: https://stackoverflow.com
[12]: https://www.raywenderlich.com/5456-in-app-purchase-tutorial-getting-started