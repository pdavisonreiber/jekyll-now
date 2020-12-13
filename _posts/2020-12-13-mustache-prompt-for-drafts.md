---
layout: post
title: Mustache Prompt for Drafts
slug: mustache-prompt-for-drafts
date: 2020-12-13 12:28:00 +0000
category: 
titlelink: 
tags: [Drafts, Mustache]
---

Today I am releasing two new [Drafts][1] actions. Originally, I had planned for these to be just one action, but as I was working on my script, I realised that part of the functionality I was building could be useful in its own right, and could easily be carved out into its own action. 

[Mustache Prompt][5] is the result of that. [Version 11.0 of Drafts][2] introduced a new `MustacheTemplate` object which allows the use of the [Mustache template system][3] within Drafts. Principally designed for use with HTML, Mustache allows for text templates to be created with placeholder variables. You tell the template what the values of all the variables are (using JSON) and it fills them in correctly for you. It can deal with variables of several different types including strings, arrays of strings, and booleans.

Mustache Prompt allows you to create Mustache templates in Drafts, and then instantiate those templates to create new drafts with the data you want. It does this by automatically detecting the variables in your template, and then prompting you to enter those values. Once you’ve entered the values, a new draft will be created with the values you entered. It’s a lot like a TextExpander fill-in snippet.

I’ve also extended the Mustache syntax to allow for variable type annotations, including dates and booleans. This ties into the prompt functionality within Drafts, so that if you annotate a data variable using the syntax `\{\{date:variable_name\}\}`, you will get a date picker in the prompt rather than a text field. Similarly, you can write `{{bool:variable_name}}` to get a toggle switch.

Furthermore[^4], you can also adjust date values by a given number of days, weeks, or months. Here are some examples of how to use that syntax:
- `{{date_variable+2d}}` adds two days to the date
- `{{date_variable-3w}}` subtracts three weeks from the date
- `{{date_variable+5m}}` adds five months to the date

The first time you declare a date, you have to use the `date:` annotation, but after that there’s no need. 

Mustache Prompt also supports arrays of strings, though there is no need to annotate here. If you use the `{{#tag}}` Mustache syntax, which can accept arrays, they will automatically be detected. In a text box, you just need to enter the values separated by commas.

Perhaps the best way to see how this all works is to [download the action][5] and then run it on this text:

> Here is a basic {{text_variable}}. 
> 
> {{bool:#boolean_variable}}
> Here is a paragraph that will only display if boolean_variable is true.
> {{/boolean_variable}}
> 
> {{^boolean_variable}}
> Here is a paragraph that will only display if boolean_variable is false.
> {{/boolean_variable}}
> 
> {{#comma_separated_strings}}
> Here is a paragraph that is repeated for each item in the array comma_separated_strings. The current item is {{.}}.
> {{/comma_separated_strings}}
> 
> Here is a date: {{date:date_variable}}.
> Here is the same date three days later: {{date_variable+3d}}
> Here is the same date one week ago: {{date_variable-1w}}
> Here is the same date in seven months’ time: {{date_variable+7m}}
> Here is another date that is offset without the original date being shown: {{date:another_date+3w}}

And just wait till you see the second action I’ve build with this.

[1]: https://getdrafts.com
[2]: https://docs.getdrafts.com/docs/misc/changelog-ios#110---gmail-outlook-integration-and-more
[3]: https://en.wikipedia.org/wiki/Mustache_%28template_system%29
[^4]: It’ll make more sense why I’ve added this when you see the next action I’ve created.
[5]: https://actions.getdrafts.com/a/1fT