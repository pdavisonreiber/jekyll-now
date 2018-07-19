---
layout: post
title: Fantastically Good Reminder Parser for Drafts 5
slug: fantastically-good-reminder-parser-for-drafts-5
category: 
titlelink: 
tags: [Drafts, Reminders]
---

Following my recent blog post about my [Fantastically Good Event Parser][1], which to my great delight was [featured on MacStories][2], I received quite a few requests to create something similar for Reminders. [Fantastical][3], which inspired the event parser, also allows you to create reminders in the system Reminders app [using natural language][4]. The interface within the app that allows you to enter calendar events also allows you to create reminders by including words like `reminder`, `task`, or `todo` in the text entered. 

I thought about replicating this functionality within my previous action, but this didn’t feel quite right in Drafts. Adding calendar events and setting a reminder are conceptually quite different kinds of action, so I decided to build a separate action just for reminders. Being separate, I also decided that requiring a keyword like `reminder` or `todo` didn’t make much sense for this use case, so I’m diverging slightly from the traditional Fantastical functionality here.

Otherwise, it works very similarly to Fantastical. Reminders can be entered in the form `Thing Tuesday 5pm !!! /p` to add a reminder called _Thing_ with a due date of Tuesday, with a reminder at 5pm that day, with high priority, and stored in my _Personal_ reminders list. Here are some details on how the parsing works:
- The name of the reminder is whatever is left when the date, time, priority, and list name are removed. [^5]
- The date and time can be entered in pretty much any normal format. Again, I’m using chrono.js behind the scenes here. Dates without times will default to having an alert at noon.[^6]
- Priority is optional and can be set as `!`, `!!`, or `!!!`.
- The reminder list is set using a forward slash followed by one or more letters from the beginning of the name of the list. If a matching list is not found, or if no list is specified, the reminder will go into the system default reminders list. 

I’m grateful to [Greg Pierce][7], the developer of [Drafts][8] for recently adding [some functionality][9] to the app’s scripting engine in [version 5.3][10] in response to [my request][11]. Without these changes, I wouldn’t have been able to make this action as fully functional as I wanted it to be. Being on the the beta channel in Slack, and now in the [Drafts forum][12], I can honestly say that Greg is one of the most responsive developers I have come across. Even when he doesn’t plan to implement a feature request immediately (or at all), he explains his reasons clearly in a way that us non-developers can understand. Thanks again, Greg.

You can find my [Fantastically Good Reminder Parser][13] in the [Action Directory][14].

[1]: https://polymaths.blog/2018/06/fantastically-good-event-parser-for-drafts-5
[2]: https://www.macstories.net/linked/fantastically-good-event-parser-for-drafts-5/
[3]: https://itunes.apple.com/gb/app/fantastical-2-for-iphone/id718043190?mt=8&uo=4&at=1001lsF2
[4]: https://flexibits.com/fantastical/help/adding-events-and-reminders
[^5]: Note that I’ve decided not to automatically remove words like `due` or `by`, so if you include these they will be added to the name of the reminder. I’d suggest omitting them.
[^6]: The default alarm time can be changed by [tweaking the script](https://twitter.com/pdavisonreiber/status/1017704853991297027). If there’s anyone who would like the parser to interpret dates given in the standard British format of dd/mm/yyyy, you can also change the `US` at the beginning of the third script step to `GB`.
[7]: https://twitter.com/agiletortoise/
[8]: https://itunes.apple.com/gb/app/drafts-5-capture-act/id1236254471?mt=8&uo=4&at=1001lsF2
[9]: http://reference.getdrafts.com/objects/ReminderList.html
[10]: http://getdrafts.com/changelog
[11]: https://forums.getdrafts.com/t/new-methods-for-reminderlist-object/1751
[12]: https://forums.getdrafts.com
[13]: https://actions.getdrafts.com/a/1MR
[14]: https://actions.getdrafts.com/a/1MR