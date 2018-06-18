---
layout: post
title: New Blog, Same as the Old Blog
slug: new-blog-same-as-the-old-blog
category: 
titlelink: 
tags: [Jekyll, Liquid, Working Copy, GitHub, GitHub Pages, Raspberry Pi, Termius, Screens]
---

To feel a real sense of ownership, sometimes you need to put something together yourself. And that’s what I recently decided to do with this blog. When I started writing here, I had things I wanted to share, and I needed a simple and straightforward way to share them. [Wordpress.com][1] was a great place to start: it let me get a domain name, customise how my site looked, and quickly get started on writing and publishing in Markdown, without having to worry about things like hosting.

More recently, however, I started to get a hankering to tinker in more detail. There were some things about my site I wanted to tweak and change, and I realised that a lot of these features were only available in the [higher price plans][2] on Wordpress. That got me searching for alternatives, and that’s when I came across [Jekyll][3].

Jekyll is what’s called a _static site generator_. At its heart, it’s a programme in [Ruby][4] that takes a directory of text files in [Markdown][5], [HTML][6], and [CSS][7], and spins them together into a bunch of web pages ready to be dropped onto a web server. The Markdown files are your blog posts, the HTML describes the layout of your posts, and the CSS says how it should be styled. The term _static_ refers to the fact that the resulting web pages don’t change after they have been generated. This is the opposite of a dynamic web page, where the information the user sees on the page is generated continuously as they move around the site, usually with reference to some database behind the scenes. In a static site, the pages are regenerated only when the site is updated, which is perfect for a blog where most updates are just new posts.

With Jekyll, adding a new post is as simple as creating a new text file in a directory. The Ruby programme runs, creates new versions of all of your pages and pushes them to a web server. You can customise the way your site looks by tweaking the CSS, and you can adjust the structure of your site by altering the HTML templates, which are supercharged with the templating language [Liquid][8]. 

Another big reason for moving to Jekyll is that my site is now incredibly portable. I’ll describe how I’m hosting it below, but because it’s just a bunch of static pages, I could host it on pretty much any web server. I need a computer to run the Ruby programme, but after that it just needs to send the files to a server and its job is done until the next update. I even managed to install Jekyll on my Raspberry Pi so it could do the processing for me, and in theory, it could even act as the web server too.


## Getting Started

The easiest way to get started with Jekyll is to use [GitHub Pages][9]. GitHub is the biggest host of source code on the web. You can store your code there, and manage it using a system called Git. Collections of code (called _repositories_) can be stored there for free if they are public, and for a small fee if they are private[^10]. For hosting a website which is by definition public, making the source files private isn’t necessary, so the free account is all you need. Jekyll itself is completely free and open-source, so along with GitHub Pages this gives you an extremely customisable and entirely free way to create and publish your own blog! The only cost is buying a custom domain name if you want one.

The best thing about using GitHub Pages is that you don’t have to worry about running Jekyll yourself or using a web server. GitHub just gives you a repository called _username_.github.io, you put your source files there, and all the processing and serving goes on behind the scenes on GitHub’s own servers. Then, as if by magic, Jekyll brings your site to life at https://_username_.github.io. Using GitHub Pages means you’re not running the Ruby programme yourself, which in turn restricts the options you have in terms of plugins. While I found [the plugins available][11] on GitHub Pages to be more then sufficient, if you really want to tinker down at the Ruby level, you will need to look at other hosting options.

You can start from scratch and create your HTML templates and CSS styling, but if like me you’re not an export on these things, it’s best to start from what someone else has done and then make it your own. On GitHub, this is a process known as _forking_. I used the wonderful [Jekyll Now][12] template by [Barry Clark][13] as my starting point. He has a [more detailed post][14] on how to do this, and there are some alternative templates he also recommends as starting points. Once you’ve done some customisation, you can hook up to your custom domain name and off you go.

## The Right Tool for the Job

I wanted a way of building and managing my site which was compatible with using an iPad as my primary computer. While I think I’ve found a great way of managing the site and publishing new posts directly from my iPad, I have to say that it was the Mac that really excelled with the job of helping me build the site.  

What I tried first was a little crazy. I would edit my source files on my iPad, push them up to GitHub, pull them back down onto my Raspberry Pi, which I controlled via my iPad using either SSH (using [Termius][15]) or VNC (using [Screens][16]). Then I would run Jekyll on the Pi, start a local web server on my network, and view the pages on my iPad. But as any programmer will tell you, it’s really important that the cycle of change the code, run the code, look at the output, change the code needs to be as fast and efficient as possible. This method was definitely not that. For one, the Pi was slow to build the site each time: it was often taking up to 30 seconds. Having to push to GitHub for every single tiny change is also not exactly how you’re meant to use it[^17].

I thought I would give it a go on the Mac instead, and this turned out to be a very good decision. As someone who would have considered themselves to have moved on from the Mac to live the iOS-only lifestyle, I have to give credit where credit’s due: it was the perfect tool for the job. My wife has an old mid-2012 MacBook Air with a battery so dead that it instantly turns off if the power is disconnected[^18], but after a recent reformatting and installation of High Sierra, it’s running pretty well. I installed [Xcode][19], [GitHub Desktop][20], and Ruby via [RVM][21] on the command line. Once you’ve got Ruby set up you can install Jekyll using these instructions. A tip if you are using GitHub Pages is to run the the command `gem install github-pages`. This installs all of the plugins that are compatible with GitHub Pages, and allows you to see your site exactly how it will appear on the web. 

So why is the Mac so good at the job of building and testing a Jekyll site? For one thing, you are working with a **lot** of windows at once. I had several Xcode windows open with various Markdown, HTML, and CSS files that I was working on, I had the terminal open to run Jekyll and the local web server, I had Safari open to preview the site and to read documentation, and I had GitHub Desktop open to keep track of changes and push them to my site. The combination of autosave in Xcode, plus Jekyll automatically regenerating the local site whenever it detected changes to the source files, plus the web server continuously running, meant I could tweak something in a file, wait a few short seconds for Jekyll to rebuild, refresh Safari and view my changes, all on the same machine. It made things _so_ much quicker. It really did give me a renewed appreciation for the Mac and the things it is really good at.

## Managing the Site from iOS

Having said all that, I still use an iPad as my primary computer, and I wanted a good way to maintain the site on iOS. I needed a good way to work with GitHub from the iPad, and by far the best app to do this from is the wonderful [Working Copy][22]. It’s a fantastic Git client with one of the best sets of keyboard shortcuts and one of the most extensive URL schemes I have come across in any app. I’ve been working on some Drafts actions which leverage this URL scheme, so that I can publish posts like this one directly from Drafts with a single button. They’re still a work in progress, so that’s probably a topic for another post.

## Tips and Tricks

Because I was running my Jekyll blog on GitHub Pages, I was somewhat constrained in terms of how I solved certain problems. For example, I wanted to have an Archive page, and while there were plenty of plugins available which use Ruby code to automatically generate an Archive page, I didn’t have the option of running these. Instead, I had to work within the bounds of what was possible within the [Liquid template language][23]. In the end, I was quite pleased with some of the solutions I came up with.

### Archive Page
With the Archive page, what I wanted was a list of year and month headings, most recent at the top, with links to each of the posts under their respective headings. I did’t want to include months were there weren’t any posts, and I didn’t want to repeat months with more than one post. Here’s the code I used:

{% raw %}
```
{% for post in site.posts %}

    {% assign current_post_year = post.date | date: "%Y" %}
    {% assign next_post_year = post.next.date | date: "%Y" %
    {% assign current_post_month = post.date | date: "%B" %}
    {% assign next_post_month = post.next.date | date: "%B" %}
    {% assign date_id = post.date | date: "%Y-%m" %}
    
    {% if post.next == nil %}
        <h1>{{ current_post_year }}</h1>
        <h2 id="{{ date_id }}">{{ current_post_month }}</h2>
    {% elsif next_post_year != current_post_year %}
        <h1>{{ current_post_year }}</h1>
        <h2 id="{{ date_id }}">{{ current_post_month }}</h2>
    {% elsif next_post_month != current_post_month %}
        <h2 id="{{ date_id }}">{{ current_post_month }}</h2>
    {% endif %}

    <p><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></p>
		
{% endfor %}
```
{% endraw %}

Basically, I print the post’s year and month, unless they are the same as the next post chronologically (which is further up the page). I also added `id` attributes to the months to act as HTML anchors. Then I made the date stamp on each post a link to {% raw %}`{{ site.baseurl }}/archive#{{ page.date | date: "%Y-%m" }}`{% endraw %}, so that the user can click on the date stamp and be taken straight to the right section of the Archive page. You can try it by clicking the date stamp at the top of this post or by [clicking here][24].

### Tags Page
I used a similar trick with my [Tags page][25], which collates all the different tags I have assigned to posts. It’s built using the following code:

{% raw %}
```
{% assign all_tags = site.posts | map: "tags" | uniq | sort_natural %}
<div class="posts">
{% for tag in all_tags %}
    <h2 id="{{ tag }}">{{ tag }}</h2>
    {% for post in site.tags[tag] %}
        <p><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></p>
    {% endfor %}
{% endfor %}
</div>
```
{% endraw %}

The first line really shows the power of Liquid as a functional language: it fetches all the posts, gets a list of their tags, removes duplicates, and sorts them into alphabetical order. Then I just loop over the tags, printing each heading – again with an `id` attribute – along with links to all of the pages that have that tag. At the end of each post, I then have the following code:

{% raw %}
```
{% for tag in page.tags %}
    {% if forloop.last %}
        <a href="{{ site.baseurl }}/tags#{{ tag }}" class="meta">{{ tag }}</a>
    {% else %}
        <a href="{{ site.baseurl }}/tags#{{ tag }}" class="meta">{{ tag }}</a>,
    {% endif %}
{% endfor %}
```
{% endraw %}

This lists the tags on the post (the last without a comma) and links them to the appropriate anchor on the Tags page. [Here’s an example][26] of a link to the tag for Notes on my Tags page.

Compared to the plugins which can do things like generate individual pages for each tag, I think I actually prefer this way of doing it. I like having single Archive and Tags pages which the user could easily do a text search on.

### Link Posts
I wanted my site to natively support link posts. I added a custom field to my post front matter called `titlelink:`, and then used the following code to turn the title into a link and add a visual indicator when there was text in this field:

{% raw %}
```
{% if page.titlelink %}
    <a href="{{ page.titlelink }}"><h1 class="entry-title">{{ page.title }} ↪︎</h1></a>
{% else %}
    <h1 class="entry-title">{{ page.title }}</h1>
{% endif %}
```
{% endraw %}

### Popover Footnotes
Jekyll supports footnotes out of the box, but I wanted to implement inline, popover style footnotes like this one. There’s a JavaScript plugin called [Bigfoot][27] that can do this, so I created a `js` directory in my repository and put the `bigfoot.min.js` file in there. It wasn’t initially clear to me, but it turned out that I also had to have jQuery in there as well for it to work. I then put the following code into the `default.hmtl` template in my `_layouts` directory:

{% raw %}
```
<script type="text/javascript" src="{{ site.baseurl }}/js/jquery-1.8.3.min.js"></script>
<script type="text/javascript" src="{{ site.baseurl }}/js/bigfoot.min.js"></script>
<script type="text/javascript">
    $.bigfoot();
</script>
```
{% endraw %}

### Images Directory
I configured my posts’ permalinks to be of the form `https://polymaths.blog[/linked]/yyyy/mm/slugified-title`, and so to make it easy to reference images inside a post, I set up the directory structure within my images folder to match this. FN So for example, if the URL of the post ends with `/2018/06/post-title` then the path of an image for that post would be `/images/2018/06/post-title/image-name.jpg`. This allows me to use a simple combination of liquid tags in my Markdown image links like so:

{% raw %}
```
![Image name]({{ site.baseurl }}/images{{ page.url }}/image-name.jpg)
```
{% endraw %}

GitHub does have a notional limit on repositories of 1GB, so at some point I’ll probably have to host my images elsewhere, but for now it’s a good solution.

## Final Thoughts

Rebuilding my blog with [Jekyll][28] has been great fun and I’ve learned a lot. It’s let me get to grips with some of the basics of web programming, which is something I’d like to develop further in the future. Along the way I’ve used some great tools, from my intrepid little Raspberry Pi, to my old but reliable MacBook Air, to the incredible [Working Copy][29] app for iOS.

Here’s to the new and improved PolyMaths. Now to get writing!

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
[21]: https://rvm.io
[22]: https://itunes.apple.com/gb/app/working-copy/id896694807?mt=8&uo=4&at=1001lsF2
[23]: https://shopify.github.io/liquid/
[24]: https://polymaths.blog/archive#2018-06
[25]: https://polymaths.blog/tags
[26]: https://polymaths.blog/tags#Notes
[27]: http://www.bigfootjs.com
[28]: https://jekyllrb.com
[29]: https://itunes.apple.com/gb/app/working-copy/id896694807?mt=8&uo=4&at=1001lsF2