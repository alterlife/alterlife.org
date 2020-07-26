---
layout: post
title:  "TypeORM REST Scaffold"
date:   2020-07-27 00:42:-- +0530
categories: Project
author: jj
---

A nodejs library built on top of expressjs. It generates an expressjs handler function that can find or create a row in a table through TypeORM in response to a GET or POST request. It's a quick way to get an express rest endpoint that directly matches a model without writing much code.

<!-- more -->

For example to GET a user, we would normally write an express handler to:

1. Find a user,
2. Return the data if available,
3. Redirect to an error page if something goes wrong.


Using typeorm-rest-scaffold, we would instead call just call typeorm-rest-scaffond's ServiceBuilder, pass it your model name and generate a handler function that will do all this with one line of code:

{% highlight javascript %}
  app.get('/:id', ServiceBuilder.findOne(User, ["id"]) ) // Route to find one user, accepting id as a parameter
{% endhighlight %}



## How to use

The first thing to do is setup a TypeORM project. With TypeORM installed, an easy way to do this is to run "typeorm init":

{% highlight shell %}
$ mkdir typeormtest
$ typeorm init
{% endhighlight %}

Next install express:

{% highlight shell %}
$ npm install --save express
{% endhighlight %}

Pull in typeorm-express-rest-scaffold as well:

{% highlight shell %}
$ npm install --save https://github.com/alterlife/typeorm-express-rest-scaffold.git
{% endhighlight %}


Finally, create a model and update the index.ts file to register the TypeORM model as a route. In the example here we use a model called "User":

{% highlight shell %}
import "reflect-metadata";
import {createConnection} from "typeorm";
import {User} from "./entity/User";
import { ServiceBuilder } from 'typeorm-express-rest-scaffold'

createConnection().then(async connection => {
  
  const express = require('express')
  const app = express()
  const port = 3000

  app.get('/:id', ServiceBuilder.findOne(User, ["id"]) ) // Route to find one user, accepting id as a parameter
  app.get('/:firstname', ServiceBuilder.findAll(User, ["firstName"]) ) // Route to all users given a first name.

  app.listen(port, () => console.log(`Example app listening at http://localhost:${port}`))

}).catch(error => console.log(error));
{% endhighlight %}

You now have a REST service that can find Users by ID and firstName.

<div style="text-align: right">
<a href="https://github.com/alterlife/typeorm-express-rest-scaffold">View Code on Github</a>
</div>



