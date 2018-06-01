---
layout: post
title: Things Parser for Drafts 5
tags: [Agile Tortoise, Drafts, JavaScript, Things]
redirect_from: "/2018/03/07/things-parser-for-drafts-5/"
---

**UPDATE:** _I’ve just released a version 2.0 of my Drafts Parser with new block-based entry and project creation features. You can read more about the update [here]({{ site.baseurl }}{% link _posts/2018-04-18-things-parser-two-point-o-for-drafts-5 %})._

I've had a lot of fun lately playing around with the beta of [Drafts 5](https://agiletortoise.github.io/drafts-documentation/). The developer [Greg Pierce](https://www.twitter.com/agiletortoise) of [Agile Tortoise](http://agiletortoise.com/) has been hard at work on a number of new features, notably developing and expanding the possibilities for [JavaScript automation](https://agiletortoise.github.io/drafts-documentation/scripting/) within the app. If you can pick up some JavaScript, there are some very powerful new things you can now do. 

For example, I wrote a [script](https://github.com/pdavisonreiber/Public-Drafts-Scripts/tree/master/Markdown%20Reference%20Link) to automatically add Markdown Links in reference style, complete with clipboard URL detection and automatically incrementing link reference numbers. You can add it to Drafts from the Drafts 5 Action Directory [here](https://drafts5-actions.agiletortoise.com/a/1C9).

Recently, I was reading [Federico Viticci’s](https://www.twitter.com/viticci) [article on MacStories](https://www.macstories.net/ios/things-automation-building-a-natural-language-parser-in-workflow/) about Things automation. The [Cultured Code](https://culturedcode.com/things/) team have [recently updated](https://www.macstories.net/stories/things-automation/) Things with [an advanced new URL scheme](https://support.culturedcode.com/customer/en/portal/articles/2803573#add-json). In addition to a lot of new URL scheme actions, they added an `add-json` command which allows you to import a bunch of tasks or projects into Things with a single URL scheme action: no jumping back and forth between apps with repeated x-callback-URLs. Federico created a [workflow](https://workflow.is/workflows/b852622a129a45ab81322b0003a7314a) which takes the content of the clipboard copied from a text editor, and assembles a chunk of JSON to send to Things. Each line of the text becomes a separate task, with special characters giving additional metadata about each task.

As part of its new scripting features, Drafts 5 added [integration](https://github.com/agiletortoise/drafts-documentation/wiki/Things) (via JavaScript) with this new `add-json` action, so this got me thinking about whether I could recreate Federico’s idea directly in JavaScript within Drafts.[^1] What I have ended up creating is a script which is completely compatible with [his syntax](https://www.macstories.net/ios/things-automation-building-a-natural-language-parser-in-workflow/#the-syntax), but expands upon it and makes it little more flexible.

First of all, it removes the need for the double backslash before the date and time of the event. I’ve built this script on top of a bit of JavaScript called [Chrono](https://github.com/wanasit/chrono). It’s a natural language parser that works for multiple languages and can detect references to dates automatically within a string. This means you can write things like “Tomorrow” or “Tuesday” anywhere in the line and it will pick that out as the day for the task. In addition, it will automatically set a reminder if (but only if) a time is also included.

In addition, I wanted my script to support all of the features that the Things `add-json` command supports, so as well as the special markup characters for projects, headings, tags, and notes, I’ve added characters for deadlines and checklist items. The full syntax is as follows:

* \#Project Name
* ==Heading
* @Tag Name
* ++Note
* !Deadline
* \*Checklist item

As with tag names in Federico’s workflow, multiple checklist items can be entered.

With this Drafts action, I can type multiple lines in the following format, and they are all simultaneously sent to the right place in Things.

> Write blog post about Drafts and Things today at 4pm #Writing ==PolyMaths @drafts @things !Friday \*drink espresso \*write \*publish

If you’re a Drafts user and want to use this script, you can only do so if you are on the Drafts 5 beta. If not, the full release of Drafts 5 should be coming soon, so watch out for that.

You can see my script [here](https://github.com/pdavisonreiber/Public-Drafts-Scripts/tree/master/Things%20Parser), and you can add it as action in Drafts by downloading [this file](https://www.dropbox.com/s/s05519g4hrljzup/Drafts%20Parser.draftsAction?dl=1).

[^1]:	Of course, it is very simple to create an action that launches Federico’s workflow directly from within Drafts. Drafts has a native “Run Workflow” action step which you can use.
