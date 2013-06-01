express-view-cache
==================

Unobtrusive solution to express framework - cache rendered page, without database requests and page rendering.
Technically it works as middleware - it tries to get and return the cached response, where the URL is the key to cached value.
It can use memory storage or [https://memcachier.com/](memcachier.com) via module [https://npmjs.org/package/memjs](memjs) as backend.
Right now it is tested in production on heroku hosting.  Feedback is welcome!

Example
==================

    var express = require('express'),
        http = require('http'),
        path = require('path'),
        cachingMiddleware = require('./../');

    var app = express();

    app.configure(function () {
        app.set('port', process.env.PORT || 3000);
        app.use(express.logger('dev'));

        app.use(cachingMiddleware(1000,{'type':'application/json'}));
        app.use('/cacheFor5sec',cachingMiddleware(5000,{'type':'application/json'}));
        app.use('/cacheFor3sec',cachingMiddleware(3000,{'type':'application/json'}));

        app.use(app.router);
        app.use(express.errorHandler());

        app.get('*', function(request,response){
            response.json({
                'Page Created At':new Date().toLocaleTimeString()
            });
        });
    });

    http.createServer(app).listen(app.get('port'), function () {
        console.log("Express server listening on port " + app.get('port'));
    });


Options
==================

    app.use(cachingMiddleware(1000,{ //cache is stored for 1000 milliseconds
        'type':'application/json', //type of returned content - see [http://expressjs.com/api.html#res.type] for details

        //driver to be used
        'driver':'memory', //development memory storage
        //'driver':'memjs', //memJS driver for managed memcache - see [https://npmjs.org/package/memjs] for details.
        //It works from the box if you ran you app at heroku hosting with Memcachier addon installed
    }));

Tests
==================

Install vows and test as usual

    $ npm install vows
    $ npm test

[![Build Status](https://travis-ci.org/vodolaz095/express-view-cache.png)](https://travis-ci.org/vodolaz095/express-view-cache)

Credits
==================

  - [Ostroumov Anatolij](https://github.com/vodolaz095)

License
==================

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2011-2013 Ostroumov Anatolij [http://teksi.ru/resume/](http://teksi.ru/resume/)



