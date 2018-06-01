---
layout: post
title: Things Parser 2.0 for Drafts 5
tags: [Agile Tortoise, Drafts, JavaScript, Things]
redirect_from: "/2018/04/18/things-parser-20-for-drafts-5/"
---

Today is release day for the wonderful [Drafts 5](https://itunes.apple.com/gb/app/drafts-5-capture-act/id1236254471?mt=8&uo=4&at=1001lsF2). There are a few great reviews out there, not least [Tim Nahumck](https://www.twitter.com/nahumck), who’s published an [amazingly comprehensive review](https://www.macstories.net/reviews/drafts-5-the-macstories-review/) over at MacStories. Also worth checking out [Rosemary Orchard](https://mobile.twitter.com/rosemaryorchard)’s review over on [her blog](https://www.rosemaryorchard.com/blog/drafts-5-review).

I haven’t been writing a review, but since joining the beta in January I have been spending a lot of time getting to grips with the app’s new action scripting capabilities. Drafts 5 allows you to write code in JavaScript to manipulate the content of drafts in almost limitless ways, including integration with other apps via URL schemes, and with web services via their APIs. The developer [Greg Pierce](https://www.twitter.com/agiletortoise) of [Agile Tortoise](http://agiletortoise.com/) has done an amazing job of abstracting away a lot of the complexity of integrating with these apps and services by creating a series of JavaScript objects that can be directly manipulated within the code. If you’re interested in how to do this, check out his comprehensive [scripting documentation](https://github.com/agiletortoise/drafts-documentation/wiki).

I went into the beta programme knowing basically zero JavaScript, and a few months later I’m pleased to say I have come out having learned a huge amount. I’ve written a few script actions so far, and with each one my knowledge of the language has steadily improved. [I wrote previously](https://polymaths.blog/2018/03/07/things-parser-for-drafts-5/) about my Things Parser, inspired by [Federico Viticci](https://www.twitter.com/viticci)’s idea for a [workflow](https://www.macstories.net/ios/things-automation-building-a-natural-language-parser-in-workflow/) that allows quick entry of multiple items into [Things 3](https://itunes.apple.com/gb/app/things-3/id904237743?mt=8&uo=4&at=1001lsF2) using natural language. It takes multiple tasks, each entered on a separate lines within a draft, with special characters denoting metadata for each task, and sends them to Things with a single x-callback-url. I built a JavaScript version of his workflow, which expanded the natural language support, and added support for additional metadata such as deadlines and checklists.

Since then, I’ve been working on developing my knowledge of the object-oriented aspects of JavaScript. While technically JavaScript is a prototype-based language rather than a class-based language, it does have support for classes. I was keen to try to build a programme based on classes, and while mind-bending at times, it was a great way to learn some object-oriented programming. The thought process of creating classes with constructors and properties and methods is a considerable mental adjustment from a more basic functional programming approach, but I’m hoping it will be the gateway to learning more advanced programming in the future.

I may write more in the near future about this learning process, and about the apps and resources I have been using, but the result is a brand new version 2.0 of my Things Parser, completely rewritten using JavaScript classes and adding several significant new features. Before I get onto those, let me just summarise the basic syntax, which I’ve changed slightly from the previous version.

* \#Project Name
* ==Heading
* @Tag Name 
* //Note
* !Natural Language Deadline
* \*Checklist Item[^1]

Each of these, and combinations thereof, can be added after the name of the task and that information will be transferred to Things. Dates and reminders are automatically detected and parsed in natural language so no special characters are required. Here’s an example:

	shopping tomorrow at 5pm #Personal
	publish blog post today #Blog ==Drafts
	presentation today !Friday #Work ==Meetings @Important *research *make presentation *follow up //Ask Bob’s opinion on this

The headline new feature is block-based entry. Previously to add a number of new tasks with the same metadata, you would need to add that information to each line. So for example you might write something like:

	task 1 today
	task 2 today
	task 3 today

Now you can just write the following:
 
	today
	task 1
	task 2
	task 3

This works with all of the metadata previously supported so even things like this are possible

	today at 5pm !Friday #Project ==Heading @Tag 1 @Tag 2 *checklist item 1 *checklist item 2 //note
	task 1
	task 2
	task 3

If a task has metadata that conflicts with the block heading, the task’s metadata wins, but it will still inherit anything that doesn’t conflict. So things like this are fine:

	#Project !Friday
	Task 1
	Task 2 !Monday
	Task 3

Task 2 will be added to `Project` but will have a different deadline to the other tasks. Multiple blocks can be entered within a single draft and should be separated by a blank line.

The other big new feature is project creation. Using the new syntax `+Project` you can create a new project and add tasks to it. It works in two different modes: in-line and block-based. With the in-line mode you can just add `+Project` to the end of any line and it will create a new project with that task as the only entry. Headings can also be created, and an area can be specified. Any other metadata is assigned to the task.

	task +Project ==Heading #Area today at 5pm !Friday

This creates a project called `Project` in `Area` with a heading and a single task under that heading. The task is assigned to today, has a reminder for 5pm, and has a deadline of Friday.

Block-based mode works in similar way with a couple of small changes: all metadata on the block heading is inherited by the new project, not the tasks, and multiple headings can be specified. Metadata must be specified for each task individually. If a task is given one of the headings specified in the block heading, it will be put under that heading, otherwise it will be assigned to the project with no heading.

	+Project today at 5pm ==Heading 1 ==Heading 2 #Area @tag
	Task with no heading
	Task under heading 1 ==Heading 1
	Task under heading 2 ==Heading 2

In this case, the date and tag will be added to the project, *not* the tasks.

It is possible to combine the project creation feature with the block-based task metadata inheritance using two blocks, one which creates the new project, and then another which adds tasks under it. So for example, if I wanted to create an important work project due on Friday with three tasks I wanted to work on today, I could do the following:

	+Project #Work !Friday @Important
	
	today #Project
	task 1
	task 2
	task 3

I hope you enjoy using my script action. If you find any bugs or unexpected behaviour, you can let me know [on Twitter](https://www.twitter.com/pdavisonreiber). For more information on Drafts 5 more generally, check out the [new site](http://getdrafts.com/). If you’re interested in finding out about what other custom actions are available, have a look at the [Action Directory](http://actions.getdrafts.com/), and if you want to talk to others about actions you’ve built or to get help, I’d encourage you to join the [Drafts Community](https://forums.getdrafts.com/).

You can download [Drafts 5](https://itunes.apple.com/gb/app/drafts-5-capture-act/id1236254471?mt=8&uo=4&at=1001lsF2) from the App Store, and you can download my [Things Parser](http://actions.getdrafts.com/a/1DV) from the Action Directory.



[^1]:	I have also added support for customising these special characters. Poke around in the script and you will see where you can change them.
