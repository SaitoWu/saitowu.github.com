---
layout: post
title: JRuby with Maven3
category: note
excerpt: Port your java programs to jruby with maven3
---

### 预备条件:

`jruby 1.6.0+`

`maven 3`

### 触发条件:

`:~$ rvm use jruby`

`Using /home/saito/.rvm/gems/jruby-1.6.2`

`:~$ gem i mvn:org.slf4j:slf4j-simple`

`Successfully installed mvn:org.slf4j:slf4j-api-1.6.2-java`

`Successfully installed mvn:org.slf4j:slf4j-simple-1.6.2-java`

`2 gems installed`


### 完成条件:

`jruby-1.6.2 :002 > require 'java'`

` => true`

`jruby-1.6.2 :003 > require 'rubygems'`

` => true`

`jruby-1.6.2 :004 > require 'mvn:org.slf4j:slf4j-simple'`

` => true`

`jruby-1.6.2 :005 > logger = org.slf4j.LoggerFactory.getLogger("maven3")`

` => #<Java::OrgSlf4jImpl::SimpleLogger:0x487bd46a>`

`jruby-1.6.2 :006 > logger.info("maven3 & jruby ")`

`27740 [main] INFO maven3 - maven3 & jruby`

` => nil `

### 意义:

  在完成与maven的整合后,唯一挡在Java开发者前面的大山就只剩下Spring了.

  不过Spring也可以通过上述方式整合:

  `:~$ gem i mvn:org.springframework:spring`

  米那桑可以大胆的在没有性能要求的地方cut掉你大部分代码了.

### 缺陷:

  gem i 的速度非常慢.(算是老毛病了.每个用ruby的公司都需要自己的mirror!

### 疑问:

  可能会选择War包部署的方式,但这其实并不是很方便.

  我暂时不知道如何install本地的jar,默认配置是会失败的,可能可以通过指定source解决.

  其他未知元素.

