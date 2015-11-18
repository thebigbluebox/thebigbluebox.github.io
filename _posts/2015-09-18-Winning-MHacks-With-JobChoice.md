---
title:  "JobChoice and Winning best in Data Analytics in MHacks"
date:   2015-9-22 9:00:00
description: What I did at MHacks and how my partner and I won Best in Data Analytics
---
# JobChoice

##### The project that I did at this year's MHack6

This year at Michigan University's 6th hackathon, MHack6, my partner (Ryan) and I set out to make a web application that made it simple for users to find which location
from your different job offers will best fit your choice of savings, comfort, and safety.

![MHACKS Logo](http://mhacks.org/images/mhacks_logo.svg)

The web application that we designed is composed of several different pieces of technology, lets list the stack now
* Node JS
* Express
* Jade
* IBM Watson Trade Off analytics

So how does all of these things work together? Lets dive in with this section by section explanation

##### What is IBM Watson Trade Off Analytics?
I am pretty much certain that you’ve heard about IBM’s Watson super computer, well if you didn’t know, it is a super computer that had the capability to understand and respond to a variety of different questions. It was able to understand questions from Jeopardy and beat the top 3 champions back in 2011. But now in 2015 IBM has opened up Watson’s intelligent analytics platform for use by everyone through simple API calls in IBM Bluemix.

At the heart of JobChoice, is the analytics platform provided by Watson, our data is feed into Watson’s tradeoff analytics and from then on Watson replies with a JSON document containing its findings. The document is then further processed by the UI to utilize its answers for the graph JS widget provided by IBM.

IBM tradeoff analytics only require a JSON recipe to brew your solution. The JSON that you send to Watson will include the following:

* Values to be returned with its format, key, how you want to optimize it (max/min), and a few presentation parameters
* Options, which are the input data that you will be sending over to Watson to be analyzed. These options should have the same parameters and should have all fields filled or Watson will be automatically discard the options with missing parameters

That’s it, these two are the primary necessary components you need to get Watson to understand your problem and to have it respond to you with a solution.

The comparison takes data from a source that we scrap and uses an web api called import.io to handle the scraping of data and sending it back to us in JSON format.

##### The setup

And to actually install Tradeoff analytics in your Node.JS application which are pretty standard for all the Node Packages, here are the steps to do so

1. Install Watson-Tradeoff analytics through npm by including this line in your package.json "watson-developer-cloud": "~0.9.3",
2. Run NPM install to download the dependencies
3. Create a folder called config and create two files within it, bluemix.js and express.js

##### Bluemix.js

Bluemix.js is the initial starting point of where IBM Bluemix service will start, and this code was copied directly from the [Bluemix](https://github.com/watson-developer-cloud/tradeoff-analytics-nodejs) example however we changed it to reference from our credentials.json file:

```javascript
'use strict';

module.exports.getServiceCreds = function(name) {
if (process.env.VCAP_SERVICES) {
  var services = JSON.parse(process.env.VCAP_SERVICES);
  for (var service_name in services) {
      if (service_name.indexOf(name) === 0) {
      var service = services[service_name][0];
      return {
          url: service.credentials.url,
          username: service.credentials.username,
          password: service.credentials.password
        };
      }
    }
}
return {};
};
```
##### Express.js

Within the express.js it contains the an express application to which will be the main where the templating engine will reside. This design was a flaw from how we structured our application from the beginning of the hackathon, a better design would have been to merge this express.js with the app.js in the root directory. However we still have this as crucial part of the web app as it connects the Jade templating engine with the frontend of the application.

```javascript
'use strict';
// Module dependencies
var express = require('express'),
errorhandler = require('errorhandler'),
bodyParser = require('body-parser');

module.exports = function (app) {

  // Configure Express
  app.use(bodyParser.urlencoded({ extended: true }));
  app.use(bodyParser.json());

  // Setup static public directory
  app.use(express.static(__dirname + '/../public'));
  app.set('view engine', 'jade');
  app.set('views', __dirname + '/../views');

  // Add error handling in dev
  if (!process.env.VCAP_SERVICES) {
    app.use(errorhandler());
  }
};
```

##### The problem model

I won’t explain too much about how the data is scrapped but only on how to create the problem and then feed the problem model and give it to Bluemix as a solution to solving it.

As our problem was to compare the different living indexes across different cities, we have a fixed number of indexes that we are comparing. With the different indexes we want certain ones to be maximized(air quality, living standards) and others to be minimized(transportation costs, crime). These will be all information included in the problem model.

##### Let’s create our problem model

The problem model is a JSON array with all your parameters, and each parameters being filled with its corresponding values. A problem model JSON is composed of a columns JSON array proceeding that are the options JSON array. The columns JSON array will contain the variables that each option possesses to be compared across each other. While the options are going to be the dataset which will contain the data to be compared.

```javascript
var model = {"subject":"ToMaximizeAandC",
"columns":[{}],
"options":[{}]}
```
A simple problem model could be where there are three variables that are being analyzed (variable A, B, and C). Our main objective for this problem is trying to maximize variables A and C). To formulate this problem for TradeOff analytics we would create a our columns JSON array like so to reflect our objectives

```javascript
"columns": [{
"key":"variableA",
"type":"numeric",
"goal":"max",
"full_name":"Variable A",
"is_objective":true,
"format":"####.##"
}, {
"key":"variableB",
"type":"numeric",
"goal":"min",
"full_name":"Variable B",
"is_objective":false,
"format":"####.##"
}, {
"key":"variableC",
"type":"numeric",
"goal":"max",
"full_name":"Variable C",
"is_objective":true,
"format":"####.##"
}],
```
From above we see that for the columns we have 3 different values, which are the corresponding variable A, B, and C. And their types are all numeric, with each having a different goal of whether to be maximized or minimized, and whether if it is part of our primary objectives or not. The key is what the options will have in its JSON document to refer it to the columns.

Within the options we will fill in 3 sample data

```javascript
"options":[{
"key":"1",
"name": "first option",
"values":{
"variableA" : 10,
"variableB" : 2,
"variableC" : 99
},{
"key":"2",
"name": "second option",
"values":{
"variableA" : 2,
"variableB" : 20,
"variableC" : 50
},{
"key":"3",
"name": "third option",
"values":{
"variableA" : 3,
"variableB" : 3,
"variableC" : 3
}]
```
And the final structure should like this

```javascript
var model = {"subject":"ToMaximizeAandC",
"columns": [{
"key":"variableA",
"type":"numeric",
"goal":"max",
"full_name":"Variable A",
"is_objective":true,
"format":"####.##"
}, {
"key":"variableB",
"type":"numeric",
"goal":"min",
"full_name":"Variable B",
"is_objective":false,
"format":"####.##"
}, {
"key":"variableC",
"type":"numeric",
"goal":"max",
"full_name":"Variable C",
"is_objective":true,
"format":"####.##"
}],
"options":[{
"key":"1",
"name": "first option",
"values":{
"variableA" : 10,
"variableB" : 2,
"variableC" : 99
}}, {
"key":"2",
"name": "second option",
"values":{
"variableA" : 2,
"variableB" : 20,
"variableC" : 50
}}, {
"key":"3",
"name": "third option",
"values":{
"variableA" : 3,
"variableB" : 3,
"variableC" : 3
}}]
}

Once we have our model set up we can initiate a call to bluemix with our data through a call to our tradeoffanalytics instance in our app.js which will return the final results in a JSON format.
