---
layout: post
title: Quick Open Action for Drafts
slug: quick-open-action-for-drafts
category:
titlelink: 
tags: [Drafts, JavaScript]
---

Just whipped up a super simple little [Drafts][1] script action which I thought I’d share. By default, Drafts opens with a blank new draft, ready to capture your thoughts. If you’ve just been working on a draft, however, it will reopen when you switch back to the app, as long as you haven’t been inactive for too long. In settings, you can adjust that time delay to whatever suits you best. When you find your own sweet spot[^2], it’s a fantastic feature which pretty much always does what you want without your having to think about it.

Just occasionally though, you end up on the wrong end of that delay. If you launch expecting a blank draft, and your previous one is still there, you just hit ⌘N. But what happens if you end up with a blank draft when you were expecting to reopen your previous one?[^4] My solution was to create a two-line script action to quickly open the most recently accessed draft[^3] with the keyboard shortcut ⌘O:
``` javascript
let recent = Draft.query('', 'all', [], [], 'accessed', true, false)[0]
editor.load(recent)
```
It’s one of the simplest script actions I’ve made, but also one that I now use frequently. You can download it now from the [Action Directory][5].

[1]: https://itunes.apple.com/gb/app/drafts-capture-act/id1236254471?mt=8&uo=4
[^2]: Five minutes for me.
[^3]: The action only works if you don’t edit the blank draft first.
[^4]: Drafts does offer a “focus mode” to prevent this from happening, so this action is just for when I forget to turn it on.
[5]: https://actions.getdrafts.com/a/1Wc