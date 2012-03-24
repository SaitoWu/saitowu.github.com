---
layout: post
title: Sinatra Extensions
category: note
excerpt: Sinatra extensions in Action
---

# sinatra 扩展:

### Sinatra编程的两种风格:

经典风格:


    require'sinatra'

    get '/' do
      "hello world"
    end


模块化风格:

    require'sinatra/base'

    class Hello < Sinatra::Base

      get "/" do
        "hello world"
      end
      
      run!
    end

### 为两种风格编写扩展:

Sinatra扩展也分为两种: 

* helper 型

* dsl 型

helper型:

    require 'sinatra/base'

    module Sinatra
      module FormatHelper
        def escape_html(text)
          Rack::Utils.escape_html(text)
        end
      end

      # make sure classic style can use this extension
      helpers FormatHelper 
    end

classic style 使用extension


    require 'sinatra'

    get '/' do
      escape_html("x > y")
    end

modular style 使用extension

    require 'sinatra/base'

    class Hello < Sinatra::Base

      helpers Sinatra::FormatHelper
      
      get '/' do
      	escape_html("x > y")
      end
      
      run!
    end

### dsl型:

    require 'sinatra/base'

    module Sinatra
      module Devise
        def authenticate!
          before {
            halt 403, "You Bastards!"
          }
        end
      end
      
      # make sure classic style can use this extension
      register Devise
    end

classic style 使用 dsl extension

    require 'sinatra'

    authenticate!

    get '/' do
      escape_html("x > y")
    end

modular style 使用 dsl extension

    require 'sinatra/base'

    class Hello < Sinatra::Base
      register Sinatra::Devise
      
      authenticate!
      
      get '/' do
      	"hello world"
      end
      
      run!
    end

两种扩展可以为sinatra扩展很多东西.

例如: 

actionpack的各种helpers. 可以移植到sinatra上面来.

sinatra自己的devise也是很容易通过扩展机制来实现的.

### Sinatra helpers 和 register 的实现机制.


    # lib/sinatra/base.rb#1772L
    # Extend the top-level DSL with the modules provided.
    def self.register(*extensions, &block)
      Delegator.target.register(*extensions, &block)
    end

    # Include the helper modules provided in Sinatra's request context.
    def self.helpers(*extensions, &block)
      Delegator.target.helpers(*extensions, &block)
    end

如果Delegator.target本身没有被覆盖.则为Application本身.

而Application的register方法如下:


    # lib/sinatra/base.rb#1681L
    def self.register(*extensions, &block) #:nodoc:
      added_methods = extensions.map {|m| m.public_instance_methods }.flatten
      Delegator.delegate(*added_methods)
      super(*extensions, &block)
    end

将扩展的public_instace_methods全部抽出来了,然后给Delegator.然后delegator通过send动态调用该method.

Sinatra的helper则是直接被include至当前module或者classic模式下面的.

### MRI vs JVM

JVM:

java version "1.6.0_29"

Java(TM) SE Runtime Environment (build 1.6.0_29-b11-402-11D50)

Java HotSpot(TM) 64-Bit Server VM (build 20.4-b02-402, mixed mode)

            user       system     total       real
    send    0.378000   0.000000   0.378000 (  0.335000)
    method  0.114000   0.000000   0.114000 (  0.114000)

MRI:

ruby 1.9.3p125 (2012-02-16 revision 34643) [x86_64-darwin11.3.0]

            user       system     total       real
    send    0.290000   0.000000   0.290000 (  0.284137)
    method  0.280000   0.000000   0.280000 (  0.281496)

不难想象JRuby下的sinatra跑的比较慢.(暂时没有openjdk7的环境, 或许在JDK7下会跑的比较快.)