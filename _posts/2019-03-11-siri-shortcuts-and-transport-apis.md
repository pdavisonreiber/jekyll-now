---
layout: post
title: Siri Shortcuts and Transport APIs
slug: siri-shortcuts-and-transport-apis
category: 
titlelink: 
tags: [Web API, Shortcuts, Siri]
---

I haven’t written anything about [Shortcuts][1] since it was released last September, so I thought it was about time I share a few of the shortcuts I’ve had fun making recently and which I’m finding myself using almost every day. All of them leverage the advantages that came with the transition from Workflow, in particular the ability to both trigger them from Siri, and for Siri to natively respond with the output of the shortcut using the `show results` action.

They’ve also given me the opportunity to try two things I’ve been meaning to play around with: the [Transport for London API][2], and the wonderful JSON parsing app [Jayson][3]. The TfL API is an amazingly comprehensive web API that gives information about public transport services all over London. I wanted to use it to build shortcuts related to common journeys I take.[^5]

## Line Status Report

The first one I built to allow Siri to tell me the status of the tube line I live on. So that I could experiment with the various URL endpoints offered by the TfL API, I built a small utility shortcut which takes a URL and uses the Jayson “View JSON in Notification” action so that I can quickly see the results by dragging down on the notification that appears. Jayson is one of the apps that has really [embraced the rich notifications][4] that were introduced in iOS 12.

![JSON Shortcut, JSON in rich notification, JSON in Jayson]({{ site.baseurl }}/images{{ page.url }}/shortcuts-and-jayson.png)

Running this shortcut with the URL [`https://api.tfl.gov.uk/Line/Mode/tube`][6] returns eleven JSON objects, each corresponding to one of the tube lines in London. For example, here is the raw JSON returned for the Bakerloo Line:
```json
{
  "$type":"Tfl.Api.Presentation.Entities.Line, Tfl.Api.Presentation.Entities",
  "created":"2019-03-05T14:58:26.59Z",
  "crowding":{
    "$type":"Tfl.Api.Presentation.Entities.Crowding, Tfl.Api.Presentation.Entities"
  },
  "disruptions":[

  ],
  "id":"bakerloo",
  "lineStatuses":[

  ],
  "modeName":"tube",
  "modified":"2019-03-05T14:58:26.59Z",
  "name":"Bakerloo",
  "routeSections":[

  ],
  "serviceTypes":[
    {
      "$type":"Tfl.Api.Presentation.Entities.LineServiceTypeInfo, Tfl.Api.Presentation.Entities",
      "name":"Regular",
      "uri":"\/Line\/Route?ids=Bakerloo&serviceTypes=Regular"
    }
  ]
}
```
Since I want to check the status of a particular line, the part I need here is the `id` key. Now I can query the URL [`https://api.tfl.gov.uk/Line/bakerloo/Status`][7], which returns the following JSON data:
```json
[
  {
    "$type" : "Tfl.Api.Presentation.Entities.Line, Tfl.Api.Presentation.Entities",
    "created" : "2019-03-05T14:58:26.59Z",
    "crowding" : {
      "$type" : "Tfl.Api.Presentation.Entities.Crowding, Tfl.Api.Presentation.Entities"
    },
    "disruptions" : [

    ],
    "id" : "bakerloo",
    "lineStatuses" : [
      {
        "$type" : "Tfl.Api.Presentation.Entities.LineStatus, Tfl.Api.Presentation.Entities",
        "created" : "0001-01-01T00:00:00",
        "id" : 0,
        "statusSeverity" : 10,
        "statusSeverityDescription" : "Good Service",
        "validityPeriods" : [

        ]
      }
    ],
    "modeName" : "tube",
    "modified" : "2019-03-05T14:58:26.59Z",
    "name" : "Bakerloo",
    "routeSections" : [

    ],
    "serviceTypes" : [
      {
        "$type" : "Tfl.Api.Presentation.Entities.LineServiceTypeInfo, Tfl.Api.Presentation.Entities",
        "name" : "Regular",
        "uri" : "\/Line\/Route?ids=Bakerloo&serviceTypes=Regular"
      }
    ]
  }
]
```
To get the human-readable status, I need to take the value from the `lineStatuses` key, then the first item of that array, then the value from the `statusSeverityDescription` key. In the shortcut I built to do this, I then have a few conditional statements to change the wording of the Siri’s response in the most common cases, so that it sounds nice and makes grammatical sense. So for example, if the API returns `Good Service` I have Siri respond with “There is currently a good service on the [line name]”, but if there are delays, she instead says “There *are* currently [type of delays] on the [line name]”. In other cases, I fall back to a default, “The current status of the [line name] is: [status]”. At some point, I should probably add some other conditionals for occasional statuses like `Part Suspended`.[^8]

![Line Status Report in Siri]({{ site.baseurl }}/images{{ page.url }}/status-report-siri.png)

[Here’s the finished shortcut][12]. The import question will ask you which line you want to use, and you just need to drag the one you want to the top of the list. Then you can add it to Siri with whatever phrase you like.

## Arrivals Information

This isn’t the case on most of the London Underground network, but on the route I take to and from work, the trains are actually relatively infrequent. I have a ten minute walk from my work to the nearest station, and so ideally I want to time things so that I arrive a minute or two before my train arrives. I wanted to build a shortcut that would allow Siri to tell me how long it is until my train arrives. To do this, I used the URL endpoint [`https://api.tfl.gov.uk/StopPoint/{id}/Arrivals`][9], with the id of the station near my work. I first had to find out what this id was, so I used the endpoint [`https://api.tfl.gov.uk/Line/{line id}/StopPoints`][10] to list all of the stations, along with their ids, on a given line. This is a case where [Jayson][3] really comes into its own. When you open some JSON data in the app, you can use the key button in the top right to display the value for a given key for each element of an array. So using the previous URL with the id for the Bakerloo line, and opening the data in Jayson, I can use the key button to display the values for the key `commonName`. This makes it much easier to find the station I want, and then drill down to identify its id.

![Jayson key tool]({{ site.baseurl }}/images{{ page.url }}/jayson-keys.png)

Armed with this information, I can now build the query to get the arrivals information I need. But since not all the trains arriving at the station go in my direction, I need to filter the results I get. Each item in the array returned from the arrivals endpoint represents an approaching train, and each has a key called `destinationNaptanId` whose value is the station id of the terminus. I wanted to filter the results using this key to reduce the list to only those going in my direction. 

In the Shortcuts app, working with lists can be a little awkward at times, and things like sorting and filtering are not natively supported. However, there’s a relatively elegant way to implement a filter operation using the `Repeat with Each` block. Within the repeat block, you add an if block, and within the if block you put a `Get Variable` action with `Repeat Item` as the variable name. In the `Otherwise` section of the if block, you put the `Nothing` action. This means that the if block passes through the input when it passes the condition and passes nothing when it doesn’t. It’s perhaps not intuitive that it works this way, but the output of a repeat block is actually a list of all the outputs produced at the end of each repetition – in this case, only the inputs which matched the condition.

Once I’ve filtered the results, I count how many there are. If zero, Siri responds with a default “No trains currently due”. If there are more than one, I take the first and find that value of of the key `timeToStation` which is given in seconds, divide by 60, and round it down to the nearest minute. This is then the value that Siri replies with using the `Show Results` action.[^11]

![Time to Next Train shortcut]({{ site.baseurl }}/images{{ page.url }}/time-to-next-train.png)

[Here’s a link to the finished shortcut.][13]

Building these shortcuts was a lot of fun, and was made much easier by the wonderful [Jayson][3] app, without which it would have involved a lot of my poring over screeds and screeds of un-indented raw JSON data. I use these actions every day to get to and from work, and being able to ask Siri “When is the next train home?” and have her quickly reply is so much better than launching an app to enter the same journey information every time.

- [Download _Line Status Report_ shortcut][12]
- [Download _Time to Next Train_ shortcut][13]

[1]: https://itunes.apple.com/gb/app/shortcuts/id915249334?mt=8&uo=4
[2]: https://api.tfl.gov.uk
[3]: https://itunes.apple.com/gb/app/jayson/id1447750768?mt=8&uo=4
[4]: https://www.macstories.net/reviews/inspecting-json-files-on-ios-with-jayson/#shortcuts-and-rich-notifications
[^5]: In the API documentation, it says, “To use the Unified API, developers should register for an Application ID and Key. Append the app_id and app_key query parameters to your requests.” In practice I’ve found this isn’t necessary for the very low volume of requests I’ve been making.
[6]: https://api.tfl.gov.uk/Line/Mode/tube
[7]: https://api.tfl.gov.uk/Line/bakerloo/Status
[^8]: One of the reasons I haven’t is that the Shortcuts app still doesn’t offer an `else if` option in conditional blocks, necessitating awkward nested blocks as a workaround.
[9]: https://api.tfl.gov.uk/StopPoint/940GZZLUMTC/Arrivals
[10]: https://api.tfl.gov.uk/Line/bakerloo/Status
[^11]: In my version of this shortcut, I put in the line status shortcut at the end with a _Run Shortcut_ action, so that Siri also tells me about any disruptions after telling me about when my train is.
[12]: https://www.icloud.com/shortcuts/a8f32ff67efc402380ed505817d3c5cc
[13]: https://www.icloud.com/shortcuts/eb47afd2e3144eaeb8d5398b1ee6970d