---
layout: post
title:  "Building a Flask Application: Intro"
date:   2019-02-09 02:10:38 -0800
categories: python vue web-development programming
---
{.subtitle}
##Why pick Flask?

{:.first} 
Flask is a Python framework designed to provide a lightweight and highly flexible web server. [Flask](http://flask.pocoo.org/docs/1.0/) sells itself by stating how unopinionated it is, how few dependancies are required and how simple it is to get developing. Unlike [Django](https://www.djangoproject.com/), which has the opposite mentality of Flask, there is no default modeling, database abstraction or form management. All that's provided is an interface to basic web server needs like requests, routing and templating - the bare bones required to create a web app. 

There are benefits to this approach - mainly, there's no requirment to implement functionality specific to Flask (for the most part). If you've worked with Django, or other web frameworks (like [Meteor](https://www.meteor.com/), or even [WordPress](https://wordpress.org/)), there's a learning curve when it comes to implememnting functionality outside the default framework. Another positive is how there are an abundance of extenstions that provide a wide range of solutions to common application problems. Flask-SQLAlchemy, for example, connects the standard SQLAlchemy to Flask, providing boilerplate logic when creating databases. 

Perhaps the main negative is to all that freedom is when you want things to work without configuration or potentional conflict. Relying on third-part support means development of core features and external code can fall out of wack - especially when major iterations are released. But this concern is mitigated when you do your dudiligence selecting plugins.

## What other tech should be used?

Selecting Flask is only one part of the puzzle when building a web app. Backend's need to be connected to a database, where many options abound. Personally, I'd look for well supported database ORMs, like SQLAlchemy. From there, I'd choose whichever makes the most sense. Postgres is a personal favourite since its FOSS and very capable - implementing the full SQL standard and many new features (like JSON). MySQL is also a popular choice, it's mostly free and has a native IDE. Outside of SQL (relational DBs) exist NoSQL databases, Mongo being the mainstream choice for most, it provides all the benefits of Non relation databases (like very flexible data stores, easy data modeling and native JSON support). Mongo also has the benefit of being very well supported, both financially and across programming languages. 

Another choice that needs to be settled is what templating engine should be picked. My [previous article](https://gabehab.github.io/me/python/vue/web-development/programming/opinion/2019/01/31/refreshing-old-stacks.html) about 