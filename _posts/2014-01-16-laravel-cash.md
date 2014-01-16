---
layout: post_page
title: Cache layer for Laravel
---

For one of my projects I've built a RESTful JSON API using the awesome [laravel framework](http://laravel.com/). This API is used by several single page applications. The most frequently used urls are `GET` routes, whose content almost doesn't change over time. So caching those responses would make sense. My first approach was to cache the responses "inside" laravel and return them later with a controller. This already led to a nice **performance boost** from around 160ms response time before to 60ms. The main advantage was that the database wasn't hit anymore but PHP still had to handle each request.

After a bit of research I figured out a way to serve content direct from memcached using nginx without delegating the request to PHP. This seamed like a much better way, so I rewrote the caching system using memcached. I could decrease the response time even more to around **3ms per request** in a local environment (this value may vary on your system).

To make this system available to everybody I created a package called [laravel-cash](https://github.com/schickling/laravel-cash). It is a simple to use cache layer for your own laravel application. This package requires laravel 4.1, memcached and the php memcached extension. You can **get started** [here](https://github.com/schickling/laravel-cash).