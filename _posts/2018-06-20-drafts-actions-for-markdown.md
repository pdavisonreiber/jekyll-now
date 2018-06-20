---
layout: post
title: Drafts Actions for Markdown
slug: drafts-actions-for-markdown
category: 
titlelink: 
tags: [Markdown, Drafts]
---

Here are a couple of [Drafts 5][2] script actions I’ve been working on recently. A while back I created [an action][3] to create Markdown links in the [reference style][1]. This allows you to use the syntax `[link text][1]` in the body of the text, and then `[1]: url` at the bottom of the document. This is nice especially in long posts for keeping the body of the text uncluttered and readable. 

The script automatically inserts the correct syntax both in the body and at the end, automatically incrementing the number each time you add a new link. If there is a URL in the clipboard, it will automatically use this. 

What I’ve done more recently is to create a new, and similar, [footnotes action][4]. They are mutually compatible, so the incrementing of the reference number is shared. If there is text selected when the footnote action is run, it is moved into a footnote[^5]. What you end up with is a mixed list of links and footnotes at the bottom of your document like this:

```
[1]: https://wordpress.com
[2]: https://wordpress.com/pricing/
[3]: https://jekyllrb.com
[4]: https://en.m.wikipedia.org/wiki/Ruby_(programming_language)
[5]: https://en.m.wikipedia.org/wiki/Markdown
[6]: https://en.m.wikipedia.org/wiki/HTML
[7]: https://en.m.wikipedia.org/wiki/Cascading_Style_Sheets
[8]: https://shopify.github.io/liquid/
[9]: https://pages.github.com/
[^10]: If you’re a teacher, however, you can apply to get a full Developer or Team plan for free, which include unlimited private repositories and unlimited users.
[11]: https://help.github.com/articles/configuring-jekyll-plugins/
[12]: https://github.com/barryclark/jekyll-now
[13]: http://www.barryclark.co
[14]: https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/
[15]: https://itunes.apple.com/gb/app/termius/id549039908?mt=8&uo=4&at=1001lsF2
[16]: https://itunes.apple.com/gb/app/screens/id655890150?mt=8&uo=4&at=1001lsF2
[^17]: Please don’t read my early commit history!
[^18]: The one time when you wish a laptop didn’t have MagSafe.
[19]: https://itunes.apple.com/gb/app/xcode/id497799835?mt=12&uo=4&at=1001lsF2
[20]: https://desktop.github.com
```

Drafts 5 also allows you to set custom keyboard shortcuts for actions, so I currently have ⌘K for inline link, ⌘⇧K for reference links, and ⌘⇧⌥K for footnotes. It makes writing in Markdown feel completely frictionless.

If you like writing in Markdown, I hope you find these actions useful. You can find them [here][3] and [here][4] in the [Action Directory][5].

[1]: https://daringfireball.net/projects/markdown/syntax#link
[2]: https://itunes.apple.com/gb/app/drafts-5-capture-act/id1236254471?mt=8&uo=4&at=1001lsF2
[3]: https://actions.getdrafts.com/a/1L4
[4]: https://actions.getdrafts.com/a/1L5
[5]: https://actions.getdrafts.com
[^5]: Like so