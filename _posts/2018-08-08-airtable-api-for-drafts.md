---
layout: post
title: Airtable API for Drafts
slug: airtable-api-for-drafts
category: 
titlelink: 
tags: [Airtable, Drafts, JavaScript]
---

I experimented last year with using Airtable as my markbook[^1]. For some reason I just find entering data into spreadsheets so incredibly dull, and the results so incredibly uninspiring, but at the same time I fully recognise that recording student data is a valuable thing to do, both for my teaching itself, and for more mundane things like writing reports. When I came across Airtable, it struck me as much more appealing, both visually and functionally. It’s a cross between a spreadsheet and a database, and it supports a large variety of different kinds of data such as files, images, and more. Its use of coloured labels and image thumbnails also makes it a much more visual experience. It also comes with a native app for iOS with native controls like switches and buttons, which makes for a really nice experience.

But as nice as the app is, both on iPhone and iPad, Airtable is primarily a web application, and the iOS app lacks a lot of the features that the web interface has. On top of that, the web interface suffers from the [curse of mobile Safari][2]. Even if you hold down on the refresh button and hit “Request Desktop Site”, basic things like scrolling just don’t work. 

There are a few features I would like to see coming to the iOS app, but for the most part, it’s just basic things that involve too many taps that I would like to see improve. For example, I have a table in my Airtable base that represents pieces of homework my students have done. When I set a new piece of homework, I want to create a whole bunch of rows with the same topic, but each associated with a different student. On the web app, you can do that quite easily by copying and pasting cells, but on iOS you need to create each new row one-by-one.

What I wanted was to be able to use the Airtable app as a visually appealing front end, but to have more power on the back end of the database to add and manipulate data from iOS. But instead of building the tools to manage my markbook, I decided instead to build the tools to build the tools. _Why go to such effort? Isn’t this just adding additional layers of procrastination?_[^3] you might quite reasonably ask – to which I would probably avoid the question and attempt to change the subject. However! I did have a few reasons for wanting to do this. 

Firstly, I have been working on my programming skills[^20], and writing a programme to interact with a database on a server is an incredibly common programming task. It brings with it common challenges such as syncing data between the local client and the remote server, formulating requests to send to the server, and parsing data from its responses into a usable format. Secondly, I wanted to create something that others could make some use of. The tools I will make for my own purposes will be far too specific for that, but building a general-purpose system has more potential use cases for a larger number of people. Thirdly, it would make my own tools easier to maintain and modify as need be, since the meta-tools from which they are built abstract away much of the complexity.[^4]

If you read my blog regularly, you will already know about the wonderful [Drafts 5][5][^6], and you will also know that it offers powerful scripting via JavaScript. Drafts is the perfect app for entering and processing data, and JavaScript is probably the language that I am most familiar with at the moment, so that’s where I decided to build it. A little while after I had begun, I saw [a post from Greg Pierce][7], the developer of Drafts, asking for input on what services people most wanted integration with in the app. Drafts already supports a large number of services, basically on two possible levels. One is as [Action Steps][8], which are little [Workflow][9]-style drag and drop blocks, with native UI switches and buttons and text fields. These have the advantage of being easy to use, and they are great for building simple actions, but they lack flexibility, as well as some of the basic things required for programming such as conditional statements, loops, and variables. The other way that apps and services can be integrated is via scripting, and here the [list of integrations][10] is much more extensive. Being part of a fully-featured JavaScript environment means that these integrations can be part of complex programmes.

There are basically two ways that Drafts can integrate with an app or service. One is via a URL scheme, where data is sent locally on the device from one app to another. The other is via a web API, where the Drafts app sends a message to a server on the web, which then responds in some way. This is the way that Airtable works. That server might also send that information back down to its own app on the same device, or on another device. Both of these abilities – to build URL schemes, and to send API requests – are both made possible within the Drafts JavaScript environment. What struck me about Greg’s post was how many different services people were asking for in the replies, and how many of them were perfectly possible but just waiting to be built. It occurred to me that instead of asking Greg to build them all himself, the number of integrations available might increase more quickly if users created them themselves. 

User-built integrations are already perfectly possible to create, and in fact a few people have created them already. Special mention goes to [Oliver Guerriat][11], who created two really useful – though not very well documented – ones which I don’t think enough people know about. [One][12] allows easy interaction with the [Bear][13] URL scheme, and [the other][14] adds easy interaction with many of Drafts own features[^15]. I’d like to see more of these in the future, with documentation as good as that in Greg’s [Scripting Reference][16]. I wonder if Greg would even consider linking to some of these user-build integrations in the Drafts Scripting Reference as optional add-ons?

I wanted to try to make something like this myself, and my Airtable API is the result. Using the ideas about Object Oriented Programming that I’ve been learning about recently, I’ve created a set of classes and methods for interacting with a base in Airtable. This is very much beta software, so I am sure there are bugs, and I am sure that people will suggest additional features that would improve it. One thing I would like to add in the near future is file uploads and downloads.

**[Here’s a link to the script][17]** in the Action Directory, and **[here’s a link to the script in GitHub][18]**. To use this, include an "Include Action" action step before your own script. [Here's a template action][22] you can use. You will also need to find out your base's endpoint and API key from the [Airtable API documentation][21].

![Airtable API documentation]({{ site.baseurl }}/images{{ page.url }}/airtable-documentation.jpeg)

If you have any suggestions for changes or improvements, or if you find a bug, please let me know on Twitter or by Email. If you know your way around JavaScript and you want to make some changes, feel free to issue a pull request on GitHub. Below is the full documentation I’ve created. I’ve aimed to stick as closely to Greg’s style as possible. 

I’m looking forward to trying this for a few other apps or services. I think next on the list might be [Working Copy][19], an app that I absolutely love and which has an amazingly extensive URL scheme. 

_[If you enjoyed this post, please check out my new Amazon recommendations page.]({{ site.baseurl}}/recommendations)_

---

# Airtable

Airtable is a web-based spreadsheet and database tool which can be used to organise a large variety of different kinds of data including text, images, files, and more. The scripting interfaces below are convenience wrappers that allow easy interaction with Airtable’s REST API.

While the Airtable API offers extensive read and write access to the data stored, it does not provide metadata about the structure of databases or the types of fields. Users will need to know this information in advance to properly interact with the database.

## ATRecord
Represents a single record in an Airtable base.

### Class Functions
- **create()** -> _ATRecord_
	- Create a new record object.
- **selectRecords(Array of ATRecord objects, field, options)** -> _Array of ATRecord objects_
	- Present a list of records to the user for them to select one or more
	- **Parameters**
		- _Array of ATRecord objects_: all records must have been added to a table and the table updated.
		- _field [string or function]_ : A string denoting the name of the field which should be used to represent the records in the selection list. Alternatively, pass a function which takes each record and returns a string to display. This can be used to combine multiple fields together to create the labels for the selection list.
		- _options [object]_: a dictionary of options with the following available keys.
			- **title** _[string]_ _(optional)_: Title to display in the prompt.
			- **message** _[string]_ _(optional)_: Message to display in the prompt.
			- **type** _[string]_ _(optional)_: Valid values are “selectMultiple”, “selectOne”, and “selectButtons”.
			- **filter** _[function]_ _(optional)_: A function to filter the records displayed.

### Properties
- **id** _[string, readonly]_
	- The unique id of the record in the Airtable base. Undefined until the record is added to a table and the table is updated.
- **table** _[ATTable, readonly]_
	- The table to which the record belongs.
- **createdTime** _[date, readonly]_
	- The time that the record was created. Undefined until the record is added to a table and the table is updated.

### Functions
- **getFieldValue(field)** -> _object_
	- Takes a _string_ with the name of the field, and returns the contents of that field.
- **setFieldValue(field, object)**
	- Takes a _string_ with the name of the field, and sets the contents of the field according to the _object_ passed.
- **getLinkedRecords(field)** -> _Array of ATRecord objects_
	- For a field which links to records in another table, this returns all of the linked records. The table containing the linked records must have been added to the base.
- **linkRecord(field, ATRecord)**
	- For a field which links to records in another table, this adds a new linked record from the given field. Existing linked records are unaffected. Note that Airtable also supports linked fields which do not allow more than one linked record.
- **update()** -> _boolean_
	- Pushes changes to the base for a record that has already been added to a table. Returns `true` if successful. 

## ATTable
Represents a table within an Airtable base.

### Class Functions
- **create(name, ATBase)** -> _ATBase_
	- Create a new table object with a given name and associated with a given base. Name must coincide exactly with an existing table on the web.

### Properties
- **name** _[string, readonly]_
- **base** _[ATBase]_
- **records** _[Array of ATRecord objects]_
	- All of the records associated with the table.
- **fields** _[Array of strings]_
	- The names of the fields associated with records in the table.

### Functions
- **addRecord(ATRecord)**
	- Add a new record to the table. Will not be pushed to the web until `update()` is called.
- **selectRecords(field, options)**
	-  Equivalent to `ATRecord.selectRecords(table.records, field, options)`.
- **update()** -> _boolean_
	- Push changes to the base. Returns `true` if successful. 

## ATBase
Represents an individual Airtable base.

### Class Functions
- **create(name)** -> _ATBase_
	- Create new base object with given name.

### Properties
- **name** _[string]_
- **tables** _[Array of ATTable objects]_
	- All of the tables associated with the base. 

### Functions
- **getRecordWithID(id)** -> _ATRecord_
	- Takes the unique id of a record within an associated table and returns the record object.



[^1]: “gradebook” for you Americans out there
[2]: https://9to5mac.com/2018/06/03/mobile-safari-is-holding-the-ipad-back/
[^3]: Spoiler: it’s layers of procrastination all the way down.
[^4]: Abstracting complexity is essentially what programming is.
[5]: https://itunes.apple.com/gb/app/drafts-5-capture-act/id1236254471?mt=8&uo=4
[^6]: [See here](https://polymaths.blog/tags#Drafts) for my previous posts about Drafts.
[7]: https://forums.getdrafts.com/t/poll-what-services-would-you-like-to-see-added-to-drafts/2021
[8]: http://getdrafts.com/actions/steps/
[9]: https://itunes.apple.com/gb/app/workflow/id915249334?mt=8&uo=4
[10]: http://reference.getdrafts.com
[11]: https://twitter.com/olivierg
[12]: https://actions.getdrafts.com/a/1Eo
[13]: https://itunes.apple.com/gb/app/bear/id1016366447?mt=8&uo=4
[14]: https://actions.getdrafts.com/a/1En
[^15]: Mind-bendingly, it seems to make the creation of actions itself scriptable!
[16]: http://reference.getdrafts.com
[17]: https://actions.getdrafts.com/a/1Nb
[18]: https://github.com/pdavisonreiber/Public-Drafts-Scripts/tree/master/Airtable
[19]: https://itunes.apple.com/gb/app/working-copy/id896694807?mt=8&uo=4
[^20]: I’ve just started reading [_Code Complete_ by Steve McConnell](https://amzn.to/2MqxIff), and I’ve already learnt a lot of good programming lessons.
[21]: https://airtable.com/api
[22]: https://actions.getdrafts.com/a/1Nc