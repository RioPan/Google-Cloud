# Twitter Toolkit Guide

The majority of setting up the Twitter Toolkit was accomplished by following [this guide](https://developer.twitter.com/en/docs/tutorials/developer-guide--twitter-api-toolkit-for-google-cloud1). The instructions below document steps where we deviated from the guide. 
This document lists all the commands to  get the Twitter Streamining Part up and running. 

## Stream Rules

In order to set up a stream, you need to add rules to the stream to filter what types of Tweets you want to receive. Twitter's [streaming rule guide](https://developer.twitter.com/en/docs/twitter-api/tweets/filtered-stream/integrate/build-a-rule) gives a create example of how to build a rule and what fields are available. We found the best fields to use were to filter on Twitter's context annotations. Twitter automatically tag's Tweets based on their internal algorithms and buckets them into categories called contexts. Within each context there are unique entities. A context is represented by an integer and the entity by a decimal place. 

For example, the company Apple is represented by the context annotation 47.10026364281. The number before the decimal, 47, is the `Brand` context and the number after the decimal is the unique identifier for Apple. The full list of contexts can be found [here](https://developer.twitter.com/en/docs/twitter-api/annotations/overview). 

The best way to retrieve the context is to use the [Tweet Annotation Extractor](https://tweet-entity-extractor.glitch.me). First, find an example of a Tweet that you want to filter for by searching on Twitter's homepage. Once you found an example Tweet, copy the link to the Tweet and paste it into the Tweet Annotation Extractor. This website will then list every annotation that has been associated with the Tweet. This is a really useful starting place to start building your rules. 

For our use case we built a rule to monitor the Apple brand and it's CEO Tim Cook with the following context annotations:  
- Apple: 47.10026364281
- Tim Cook: 131.10026364281

The rule we used to add to the stream was:  
`curl -X POST 'https://api.twitter.com/2/tweets/search/stream/rules' -H "Content-type: application/json" -H "Authorization: Bearer <<bearer token>>" -d '{"add":[{"value":"context:47.10026364281 OR context:131.10026364281 OR lang:eng"}]}'`
The final part of this rule limited our search to Tweets in English. 

That summarises our approach for connecting to the Twitter Stream. Every other step follows the guide mentioned above.
