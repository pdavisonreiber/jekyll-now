---
layout: post
title: Things Parser 3 for Drafts
slug: things-parser-3-for-drafts
date: 2020-12-13 21:25:03 +0000
category: 
titlelink: 
tags: [Drafts, Things, TaskPaper, Mustache]
---
{% raw %}
Following on from my previous post about [Mustache Prompt][1], I’m excited to announce a brand new version of my [Things Parser][16]. Thanks to its being [mentioned on MacStories][2] along with my [Fantastically Good Event Parser][3], it has become not only the most popular Drafts action that I have built, but the most popular Drafts action in the [whole directory][5] – a fact which creates in me an admixture of equal parts pride, imposter syndrome, and obligation. 

The imposter syndrome comes from the fact that I taught myself JavaScript solely [because of and through Drafts][7][^4]. It’s a little disconcerting when something you cobbled together for yourself quickly turns into a tool that many other people use daily to do their work or organise their lives. That creates a sense of obligation to maintain it, fix bugs, and reply to support emails. There’s of course something nice about that, and when people email me about an issue they’ve had, they are unfailingly kind and complimentary. But when your code starts getting complicated or you haven’t looked at it in a while, it becomes difficult to maintain. Sometimes you realised that bugs run deeper than you thought, and would require a complete overhaul of the script.

That’s the point I’d got to with [Things Parser 2][14]. There was a lot of messy [RegEx][6] in there and it was getting difficult to reason about. Fundamentally, the problem was that I – an amateur, self-taught programmer – was trying to create a _parser_. As I learnt more about programming, I realised that parsers were a whole category of programs, and that they had a vital role in compilers, the programs that interpret source code and turn it into machine code. Suddenly the idea of trying to write a parser myself seemed less wise! 

For [Things Parser 3][16], there are a couple of big changes. The first is that I didn’t write the parser part of it. My script processes the output of the parser and transforms the data into a format that [Things][9] can understand. The second big change – which I am a little nervous about, but I hope people like – is that I have completely changed the syntax. No, I have not created another custom syntax; instead I decided to use the popular and well-supported [TaskPaper format][8]. 

So why the change? From the feedback I was getting on the action, it seemed like a lot of people were using it for the purposes of templates. They would keep a draft around that they could export into Things to start a new project or create a task with the same checklist every time. Perhaps a lot of people still use it for quick entry of one or two tasks, but the impression I got was that there was a desire for more features to support templates. TaskPaper made perfect sense for this. 

From its beginnings in a single app, the TaskPaper format has become the Markdown for task management: a standard plain-text format compatible with a number of apps. [OmniFocus][10], for example, has for a long time supported importing templates in TaskPaper format, and Drafts itself offers a syntax highlighting option for TaskPaper documents. All things considered, it seemed like the obvious choice. I’d also considered moving to OmniFocus just for the ability to import TaskPaper, but I prefer how Things looks and its keyboard shortcuts are now firmly lodged in my muscle memory. If I could build this functionality myself, I could have the best of both worlds.

After deciding I really didn’t want to write my own TaskPaper parser, I did some research online and found that [Birch Outline][11], the parser used by the actual TaskPaper app had been open sourced five years ago. I was able to pull out the JavaScript part I needed and add it on to the front of my script. It reads the text and derives the structure of the document, and then my script takes over and processes this into the right format for Things.

Things Parser 3 supports almost all the attributes that that Things own [`json` command][12] does in its url scheme. Here are some examples to show what’s possible.

### Tasks
```
- Tasks outside projects
- with date @when(2020-12-13)
- alternatively @defer(2020-12-13)
- with natural language date @when(tomorrow)

- with deadline @deadline(2020-12-25)
- alternatively @due(2020-12-25)

- Canceled task @canceled
- British cancelled task @cancelled

- Tasks can be @done
- Or @completed

- Tasks can be assigned to a @list(name)
- alternatively @project(name)
- or using list ID @listID(id)
- and within a project to a @heading(name)

- Tasks can have a @tag(name)
- Or more than one @tags(tag1, tag2, tag3)

- Tasks can have notes
	which are indented text
- And subtasks
	- which are indented tasks
	- like this
```

### Projects
```
Projects can have dates too: @when(today)
	- also supports @defer(today)

They can have deadlines: @deadline(2020-12-25)
	- also supports @due(2020-12-25)

They can be: @canceled
	- or if they are British @cancelled

They can be: @completed
	- or @done

They can be assigned to an: @area(name)

Or using an id: @areaID(id)

They can have a: @tag(name)
	- or multiple @tags(tag1, tag2, tag3)

They can have notes:
	which are indented text

They can obviously have tasks:
	- like this one
	- and this one

But they can also have headings:
	which are written like sub-projects:
		- and tasks within the headings will be added to those headings
```

In addition to all that, you can also use variables in the form `{{variable}}` which you will be prompted for when you run the script if they are present. For details on advanced use of variables, including dates and time offsets, see my post on the [Mustache Prompt action][15], whose functionality is built in to Things Parser 3.

You can [download Things Parser 3][16] from the Drafts action directory.[^13]

{% endraw %}

[1]: https://polymaths.blog/2020/12/mustache-prompt-for-drafts
[2]: https://www.macstories.net/linked/fantastically-good-event-parser-for-drafts-5/
[3]: https://polymaths.blog/2018/06/fantastically-good-event-parser-for-drafts-5
[^4]: I’ve no idea how you write JavaScript for a webpage, though I am told it is a popular application of the language.
[5]: https://actions.getdrafts.com/drafts_actions?order=popular
[6]: https://en.wikipedia.org/wiki/Regular_expression
[7]: https://scripting.getdrafts.com
[8]: https://www.taskpaper.com/guide/getting-started/
[9]: https://apps.apple.com/gb/app/things-3-for-ipad/id904244226
[10]: https://apps.apple.com/gb/app/omnifocus-3/id1346190318
[11]: https://github.com/jessegrosjean/birch-outline
[12]: https://culturedcode.com/things/support/articles/2803573/#for-developers
[^13]: For fans of the old action, [Things Parser 2](https://actions.getdrafts.com/a/1fU) is still available on the action directory but will no longer be maintained.
[14]: https://actions.getdrafts.com/a/1fU
[15]: https://polymaths.blog/2020/12/mustache-prompt-for-drafts
[16]: https://actions.getdrafts.com/a/1DV