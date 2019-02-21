---
layout: post
title:  "Build a URL Shortener with Node + Mongo: All things Models"
date:   2019-02-13 01:10:48 -0800
categories: nodejs programming web-development code mongodb
---

{:.first} 
A long while ago, many many moons before tonight's, I built a [NodeJs](https://nodejs.org/en/) application that shortened URLs called [Lyt.Haus](https://lyt.haus). Conceptually, it's simple and unoriginal - with hundreds, if not thousands of sites 
providing similar functionality. So why did I bother putting the hours into it? Simply put, I wanted to be able to build a project that had a realistic goal that could be accomplished from scratch in a managable timeframe, and also be used by me (and anyone else) when finished.

Looking back, I think it was a great use of time and allowed me to get down and dirty with with a full JS stack (MEEN, Mongo, Express, EJS*, Node), while also providing tangable proof I could code. I'd recommend any beginner, novice or intermediate programmer looking for a portfolio booster to give it a shot. 

*EJS is a templating engine that I currently am refactoring to Vue

## The Magic Sauce

I'll skip over most of the boring part most authors include (the project setup and enviroment-specific stuff) and jump write into some good ol' code. But first, lets get our [bering's strait](https://en.wikipedia.org/wiki/Bering_Strait) (sea what I did there?) and look at a file overview:

*views*  -- index.ejs  -- accounts.ejs
 
*routes* -- index.js -- accounts.js -- users.js
 
*models* -- users.js -- urls.js
 
app.js

*middleware* -- permissions.js
 
*credentials* -- secretstuff.js
 
package.json


It's a simple enough layout, very old school MVC, which is what its based on. Since my first go at the project wasn't with Vue, it meant using a templating engine native to Express. In this case I used EJS, instead of Pug/Jade beacuse I prefered the syntax, also Pug is definetly the normal. 

The `routes` and `models` directory is real meat and potatoes of the code. The models are straightforward, using Mongo makes life very flexible when developing on the fly. Representing a shortened URL looks like this: 

{:.code}
```JavaScript
// models/urls.js - a schema for the shortened URL

var mongoose = require("mongoose");
var uniqueValidator = require("mongoose-unique-validator");
var Schema = mongoose.Schema;
var safeBase = require("urlsafe-base64"); //need special Base64 so its url friendly, ie slashes omitted

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


urlSchema.virtual("shortURL").get(function() {
  return "/" + this.id; //easily get the url of a short link
});

urlSchema.plugin(uniqueValidator); //make sure to include our plugin to validate record
```

Theres nothing particularly special about the url records, the schema defines the basic info required. 

Now for the secret sauce - how the shortener actually functions. There's two parts, first we define a counter schema, a record that updates and counts how many urls are shortened. It looks like the following:

{:.code}
```JavaScript
var CounterSchema = Schema({
  _id: { type: String, required: true },
  seq: { type: Number, default: 0 }
});
var counter = mongoose.model("counter", CounterSchema);
```

The sequence attribute is updated for every URL, its a centeralized place to record and generate the next unique (and short) identifier. It also allows the potential of creating a more complicated URL shortener that can tie into user accounts, specific domains, etc. But for now, it just counts.

{:.code}
```JavaScript
// models/urls.js
// ...

urlSchema.pre("save", function(next) {
  var doc = this;
  var randomURLSafeBase64;
  counter.findByIdAndUpdate(
    { _id: "entityId" },
    { $inc: { seq: 1 } },
    { upsert: false, new: false },
    function(error, counter) {
      if (error) return next(error);
      var newid = safeBase.encode(Buffer.from(counter.seq.toString()));
      doc._id = newid.toString();
      next();
    }
  );
});
```

The meat and the potatoes! Here is where we translate the counter that we created above into a base64, unique id that is also not very long. Now there are various ways of doing this, like using the default id associated with a mongo record, but that's a pain to type, and if truncated, it can't be guarnteed unique. Moreover, this way ties back the mongo record's id to the short url id, making them the same thing - which is kinda nifty and saves some space. 

All in all, when creating the models for a url, think simple, and rely on native approaches like above. This has been a short overview of how a mongo schema can look, next we'll dive deeper into the routes and logic behind generating URLs and stoppinf spammers with Captcha! 




