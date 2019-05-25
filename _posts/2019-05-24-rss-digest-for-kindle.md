---
layout: post
title: RSS Digest for Kindle
slug: rss-digest-for-kindle
date: 2019-05-24 09:46:25 +0100
category: 
titlelink: 
tags: [Kindle, RSS]
---

A shortcut I made ages ago that I’ve been meaning to share. I like reading RSS, and I like reading on my Kindle, so I decided to try and make a shortcut that could turn a list of RSS feeds into a daily digest of articles I could read on my Kindle.

The shortcut leverages the _Send to Kindle_ share sheet extension which is able to accept PDFs and convert them to Kindle format. Once the shortcut has run, the share sheet will appear. Select _Send to Kindle_ and then on the dialogue box that appears hit _Send_. You will then be asked whether you want to convert the PDF to Kindle format, and you should hit _Yes_.

There were a couple of interesting challenges building this shortcut. One was dealing with block quotes, which the Kindle format simply turned into normal text, making articles containing them confusing to read. Having no idea how the Kindle app does the PDF to Kindle conversion, I wasn’t able to find a way of having the resulting document display indentation or a vertical line. My compromise was to just turn all block quotes into italic text, and the result is actually pretty good.

The other challenge was a feature I decided to add while writing this post. On iOS my current RSS reader of choice is [Unread][2]. It’s an app I’ve always liked because it just gets all the usual UI elements out of the way and lets you read uninterrupted, and the controls to the app are a rather elegant selection of gestures. But I’ve been playing around with [Fiery Feeds][3] recently because it allows feed subscriptions to be added within the app. Using Unread means I have to deal with the [Feedly website][4], which is not great and downright impossible to use on an iPhone. Fiery Feeds and several other apps allow you to export OPML, so I wanted to offer support for that as a convenient way to use this shortcut with your existing feeds. I cobbled together some regex which seems to do the job. [^1]

I had a half-working version of this shortcut that also included articles that were linked to from link posts, but hit a brick wall in making that work in a satisfactory way. Different blogs just don’t seem to be consistent in the way they indicate which posts are link posts and what the linked URL is. If anyone has any ideas how to make this work, [let me know][5].

You can download the shortcut with [this link][6].

[^1]: I wish the JavaScript action step in the Shortcuts App could be used outside of a webpage, in which case I’m sure there would be a more elegant way to parse that information.
[2]: https://itunes.apple.com/gb/app/unread-rss-reader/id1252376153?mt=8&uo=4
[3]: https://itunes.apple.com/gb/app/fiery-feeds-rss-reader/id1158763303?mt=8&uo=4
[4]: https://feedly.com/i/welcome
[5]: https://mobile.twitter.com/pdavisonreiber
[6]: https://www.icloud.com/shortcuts/e20cab9dc28b49708ce698920795f40f