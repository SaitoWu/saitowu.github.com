---
layout: post
title: Ruby off Rails
category: note
excerpt: rack, sinatra, padrino, etc...
---

Rack
====

> minimal interface for web servers


`gem i rack`

`require'rack'`

### Rack handler

`Rack::Handler.constants`

> A Rack application is an Ruby object (not a class) that responds to call. It takes exactly one argument, the environment and returns an Array of exactly three values: The status, the headers, and the body.

### Ruby object that responds to call

`object`

`lambda or proc`

`method`

`->{}.call`

> It takes exactly one argument, the environment and returns an Array of exactly
three values: The status, the headers, and the body.

`->(env){[200, {}, [Hello Saito]]}`

`Rack::Handler::Thin.run ->(env){[200, {}, ["Hey Saito"]]}`

### Middleware

    class Decorator
    def initialize(app)
    @app = app
    end

    def call(env)
    status, headers, body = @app.call(env)
    [status, headers, body]
    end
    end

`Rack::Handler::Thin.run Decorator(->(env){[200, {}, ["Hey Saito"]]})`

> Rack middleware is also a rack application.


Sinatra
=======

> Sinatra is a DSL for quickly creating web applications in Ruby with minimal effort

    require'sinatra'

  get '/' do
    'Hello saito!'
  end

### REST, [RFC-2616](http://www.ietf.org/rfc/rfc2616.txt)

    get, post, put, patch, delete, options

[Rails pull request 505](https://github.com/rails/rails/pull/505)

### Sinatra-contrib

    sinatra/reloader
    sinatra/namespace
    sinatra/respond_with
  …

Padrino
=======

> the Godfather of Sinatra

### Key Features

    Admin
  Helpers
  Generators
  Mountable

> Padrino is another Rails


Others
======

…

# Sponsored by yavaeye.com