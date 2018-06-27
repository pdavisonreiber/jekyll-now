---
layout: post
title: Fantastically Good Event Parser for Drafts 5
slug: fantastically-good-event-parser-for-drafts-5
category: 
titlelink: 
tags: [Drafts, Fantastical, JavaScript]
---

I used to have [an action][6] in [Drafts 5][5] for adding calendar events using [Fantastical][7], the favoured calendar app of many because of [its ability][8] to interpret events entered in natural language. Fantastical allows you to enter things like _Meeting on Thursday at 5pm_ and then generates a calendar event by parsing out the information you provided. Compared to tapping on dialog boxes and scrolling date pickers in other calendar apps, it feels fast and easy.

The way the action works is to take each line of the draft and run it through the [Fantastical URL scheme][9]. Sometimes I would run this action with a _lot_ of different events[^1] which meant watching as my iPad madly bounced back and forth from Drafts to Fantastical. If I remembered, I would put the two apps in split view beforehand to speed things up, but even with the `add=1` flag set in the URL scheme to avoid having to confirm each entry, Fantastical still shows an animation each time an event is entered, which slows things down.

The way apps like Fantastical actually integrate with the system calendar in iOS is via an API which allows direct manipulation of calendar events. You may have seen the _Allow app to access the Calendar?_ prompt when first launching apps which use this. Drafts integrates this API into its [scripting capabilities][10], and so it occurred to me recently that perhaps I could build a similar functionality within Drafts using JavaScript. This would allow me to use the system calendar app, which I prefer aesthetically over Fantastical, while retaining the ability to enter events in natural language.

What I’ve ended up creating has almost all of the same functionality as Fantastical, but since it does not rely on launching an external URL scheme, is considerably faster. You can enter multiple events, each on a different line, and have them all instantly added to your calendar without even launching another app. As with my [Things Parser][11], I’ve leveraged the [chrono.js][12] library to do the natural language date parsing. My action supports the following things:
- Dates and times entered in natural language[^4]
- Locations entered in the form `at location` or `in location`
- Durations entered in the form `30 minutes` or `2 hours`[^2]
- Alerts entered in the form `alert 30 minutes` or `alert 1 hour`
- The specific calendar where the event should be created in the form `/family` or simply `/f`[^3]

Unfortunately, my script doesn’t yet support creating recurring events as Fantastical does. This currently isn’t built in to the [scripting capabilities][13] of the `event` object in Drafts, but [I am hoping][14] this might be added soon. When it is, I’ll be sure to update this action to support entering recurring events.

You can find my [Fantastically Good Event Parser][15] in the [Action Directory][16].

[^1]: Each term, I used it to take a plain text version of my school timetable and add it to my calendar.
[^2]: Note that the space is optional and that minutes and hours cannot be mixed. Minutes can be written as `minutes`, `minute`, `mins`, `min`, or `m`. Hours can be written as `hours`, `hour`, `hrs`, `hr`, `h`.
[^3]: Your calendars will be searched for the one beginning with the string you’ve entered. If you have two calendars with the same first letter, such as `tutoring` and `timetable`, try being slightly more specific by writing `/tu` or `/ti`. If no calendar it specified, the event will be added to the default calendar.
[^4]: Events given with a date but without a time are assumed to be all-day events. Events with a given start time but no end time are assumed to be one hour long.
[5]: https://itunes.apple.com/gb/app/drafts-5-capture-act/id1236254471?mt=8&uo=4&at=1001lsF2
[6]: https://actions.getdrafts.com/a/1ED
[7]: https://itunes.apple.com/gb/app/fantastical-2-for-ipad/id830708155?mt=8&uo=4&at=1001lsF2
[8]: https://itunes.apple.com/gb/app/fantastical-2-for-ipad/id830708155?mt=8&uo=4&at=1001lsF2
[9]: https://app-talk.com/#fantastical-2
[10]: http://reference.getdrafts.com/objects/Calendar.html
[11]: {{ site.baseurl }}{% post_url  2018-04-18-things-parser-two-point-o-for-drafts-5 %}
[12]: https://github.com/wanasit/chrono
[13]: http://reference.getdrafts.com/objects/Event.html
[14]: https://forums.getdrafts.com/t/scripting-support-for-recurring-events/1707
[15]: https://actions.getdrafts.com/a/1Lk
[16]: https://actions.getdrafts.com