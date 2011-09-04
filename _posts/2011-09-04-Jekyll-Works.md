---
layout: post
title: Jekyll Works
category: note
excerpt: Jekyll 5 W 1 H
---

### Jekyll做了什么?

Jekyll是一个静态站点生成系统, 与之类似的系统还有toto.

[jekyll](https://github.com/mojombo/jekyll) vs  [toto](https://github.com/cloudhead/toto)
  
[octopress](http://octopress.org/) 也是建立在jekyll的基础上.
  
### Jekyll可以做什么?

Jekyll被广泛用于博客系统, Jekyll + [Disqus](http://disqus.com/) 就可以完美实现了.
  
### Jekyll支持什么?

Jekyll支持markdown textile作为post的parser.其本身的layout使用了shopify的[Liquid template](http://www.liquidmarkup.org/) .
  
Liquid template是一个出色的模版引擎,除了能渲染模版外还支持两个特色功能使他跟别的引擎进行区分.
  
### Jekyll有什么缺陷?

如果Jekyll有什么缺陷? 那可能就是其内部绑定了Liquid作为模版引擎.
  
在现在大家广泛采用tilt作为通用interface的情况下,这种形式的绑定有些诡异.
  
由于octopress是基于Jekyll的,所以也只支持Liquid template.
  
ps: Liquid 的风格是<% %>做条件语句,{{ "{{ " }}}}做代入(类似mustache).html原生标签不能简写.

### Jekyll怎么改进?

tilt是一个通用的模版引擎接口,支持数十种模版的解析和渲染工作.包括textile和markdown.
  
当你install sinatra之后,他所依赖的gem就是tilt.
  
tilt的使用方式有两种:

    Tilt.new('templates/foo.liquid').render
    
or

    Tilt::LiquidTemplate.new('templates/foo.liquid').render
    
第二种方式的变种:

    Tilt::LiquidTemplate.new { "hi {{ "{{" }} name }}" }.render 'name'=>'Saito'
  
好吧,我们即将使用第二种方式来改进Jekyll.至少将interface变成tilt调用,而非直接使用:
   
    Liquid::Template.parse( "hi {{ "{{" }} name }}" ).render 'name'=>'Saito'
  
这种差距是显而易见的,这样我想起了slf4j.我相信tilt的作者是被slf4j折磨过的. = =
  
hack first step:
  
jekyll.rb #=> 
  
    require 'tilt'
  
hack second step:
  
lib/jekyll/convertible.rb
      
at line 76 and line 94
      
    -          self.output = Liquid::Template.parse(self.content).render(payload, info)
    +          self.output = Tilt::LiquidTemplate.new { self.content }.render self, payload

将Liquid template更换为别的引擎:
  
    Tilt::HamlTemplate.new
    
ps: 更换后需要重新port layout与index的源码到你改写的模版引擎中去.
  
同理: markdown和textile也可以用tilt来进行替换.
  
remember: 
  
    rake test
  
我们发现并非所有的test都过了,察看一下原因可以发现所有的failure几乎都跟date相关.或者跟tags相关.
  
原因就是Jekyll在页面上对变量做了一次pipe.从而规范化date表述.
  
而tags则是Liquid的特性.
  
在模块设计良好的系统里面,我们发现所有的模块似乎都可以轻松替换.这才是我们最大的收获.
  
### Jekyll改进版损失了什么?
  
上面提到了Liquid的两个特性,在这里给出解释.

由于tilt的通用,我们将要损失掉Liquid的tags跟filter特性.他们实际上是非常好用的.
  
代码的部分包括在lib/tags 跟 lib/jekyll/filter.rb中.这些东西将无用化..

可以看到在我们的改进中,损失了info这个变量,而info本身则是支持tag和filter的出处.

tags可以让用户自定义template中的标签.来实现自己的template语法.
  
而filter可以实现类似unix的pipe.

### Jekyll改进版比Jekyll更好?

或许答案不是肯定的,因为我们损失了tags跟filter两个非常好用的特性.
  
而我们得到了使用自己熟悉模版引擎的机会.合适与否,只能自我评断了.
  
