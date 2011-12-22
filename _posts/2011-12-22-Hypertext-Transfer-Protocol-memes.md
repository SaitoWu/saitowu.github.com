---
layout: post
title: Hypertext Transfer Protocol memes
category: note
excerpt: idempotent patch delete verb
---

### Idempotent

    9.1.2 Idempotent Methods
    The methods GET, HEAD,PUT and DELETE share this property.

9.1.2小节里面表示幂等方法是这么5个.

### Delete

    9.7 DELETE
    A successful response SHOULD be 200 (OK) if the response includes an entity describing 
    the status, 202 (Accepted) if the action has not yet been enacted, or 204 (No Content)
    if the response is OK but does not include an entity.

9.7小节, delete正确且牛逼的返回应该是: 1) 带entity返回 => 200, 异步还未处理结束 => 202, 不带entity返回 => 204 这个有人用对过么?

### Patch

    19.6.1.1 PATCH
    The PATCH method is similar to PUT except that the entity contains a list of differences
    between the original version of the resource identified by the Request-URI and the desired
    content of the resource after the PATCH action has been applied. The list of differences
    is in a format defined by the media type of the entity (e.g., "application/diff") and MUST
    include sufficient information to allow the server to recreate the changes necessary to
    convert the original version of the resource to the desired version.

19.6.1.1小节给出了这么一个addtional的请求方法,跟put的区别是patch会返回diff而不是整个entity.所以put是幂等的,patch则可幂等可不幂等..所以就不是幂等的...rails 4在还在纠结该怎么映射patch跟put在actionpack的update上.纠结着呢..

### thread

另: 看thread学知识: [github rails pull-request 505](https://github.com/rails/rails/pull/505)