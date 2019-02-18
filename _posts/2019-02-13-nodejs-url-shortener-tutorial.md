---
layout: post
title:  "Build a URL Shortener with NodeJS: Tutorial 1"
date:   2019-02-13 01:10:48 -0800
categories: nodejs programming web-development code
---

{:.first} 
A long while ago, many many moons before tonight's, I built a [NodeJs](https://nodejs.org/en/) application that shortened URLs called [Lyt.Haus](https://lyt.haus). Conceptually, it's simple and unoriginal - with hundreds, if not thousands of sites 
providing similar functionality. So why did I bother putting the hours into it? Simply put, I wanted to be able to build a project that had a realistic goal that could be accomplished from scratch in a managable timeframe, and also be used by me (and anyone else) when finished.

Looking back, I think it was a great use of time and allowed me to get down and dirty with with a full JS stack (MEEN, Mongo, Express, EJS*, Node), while also providing tangable proof I could code. I'd recommend any beginner, novice or intermediate programmer looking for a portfolio booster to give it a shot. 

*EJS is a templating engine that I currently am refactoring to Vue

## The Magic Sauce

I'll skip over most of the boring part most authors include (the project setup and enviroment-specific stuff) and jump write into some good ol' code. But first, lets get our [bering's strait](https://en.wikipedia.org/wiki/Bering_Strait) (sea what I did there?) and look at a file overview:

views \
 
 -- index.ejs
 -- accounts.ejs
 .... etc ....
 
routes \
 
 -- index.js
 -- accounts.js
 -- users.js
 
models \
 
 -- users.js
 -- urls.js
 
app.js

middleware \
 
 -- permissions.js
 
credentials \
 
 -- secretstuff.js
 
package.json
... npm downloads, etc ...


It's a simple enough layout, very old school MVC, which is what its based on. Since my first go at the project wasn't with Vue, it meant using a templating engine native to Express. In this case I used EJS, instead of Pug/Jade beacuse I prefered the syntax, also Pug is definetly the normal. 

The `routes` and `models` directory is real meat and potatoes of the code. The models are straightforward, using Mongo makes life very flexible when developing on the fly. Representing a shortened URL looks like this: 
{:code}
```JavaScript
var mongoose = require("mongoose");
var uniqueValidator = require("mongoose-unique-validator");
var Schema = mongoose.Schema;
var safeBase = require("urlsafe-base64"); //need special Base64 so its url friendly, ie slashes omitted

// urls.js - a schema for the shortened URL
// create a schema for our links
var urlSchema = new Schema({
  _id: {
    type: String,
    index: true
  },
  long_url: {
    type: String,
    required: true
  },
  short_url: {
    type: String,
    trim: true,
    index: true,
    unique: true,
    sparse: true
  },
  date: {
    type: Date,
    default: Date.now
  },
  count: { //simple analytics tracking clicks to long url FROM short url
    type: Number,
    required: true,
    default: 0
  },
  categories : [{ //allows users to add a list of cat's to url for dashboard
    type: String,
    ref: "Url.categories.name"
  }]
});

```


