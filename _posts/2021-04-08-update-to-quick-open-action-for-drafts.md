---
layout: post
title: Update to Quick Open Action for Drafts
slug: update-to-quick-open-action-for-drafts
date: 2021-04-08 16:37:03 +0000
category: 
titlelink: 
tags: []
---

I've just updated my [Quick Open action](https://actions.getdrafts.com/a/1Wc) for [Drafts](https://apps.apple.com/gb/app/drafts/id1236254471) which I previously wrote about [here](https://polymaths.blog/2019/05/quick-open-action-for-drafts). One of Drafts' distinctive features is that by default it opens to a new blank draft after you've been away from the app for a certain period of time. It's perfect for the app's emphasis on quick and easy capture. Occasionally though, you just want to get back to the draft you were previously working on, even if you've gone over the usual timeout period, and I wanted to have `⌘O` as the keyboard shortcut for this. 

While my previous action did the job, there was always a slight delay before the previous draft popped up. At the time I originally wrote the script, the only way I could get it to work was to get a list of all drafts, sort by date, and then get the first one in the list, which I imagine gets slower the more drafts you have. I wanted to see if I could fix that delay, so I started trawling throught the [Drafts Script Reference](https://scripting.getdrafts.com) to see if I could find a better way. It turns out that [version 20.0](https://docs.getdrafts.com/docs/misc/changelog-ios#200) of the app introduced a new [`editor.recentDrafts`](https://scripting.getdrafts.com/classes/editor#recentdrafts) property, which is exactly what I needed. The script is now a one-liner:
```js
editor.load(editor.recentDrafts[0])
```

Simple but effective. You can [download it from the Drafts Directory](https://actions.getdrafts.com/a/1Wc).
