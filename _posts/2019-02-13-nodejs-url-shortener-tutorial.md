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

I'll skip over the boring part most authors include (the project setup and enviroment-specific stuff) and jump write into some good ol' code. But first, lets get our [bering's strait](https://en.wikipedia.org/wiki/Bering_Strait) (sea what I did there?) and look at a file overview. 

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
