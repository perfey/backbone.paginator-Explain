backbone.paginator-Example
==========================

backbone.paginator是backbone的分页组件，功能很稳定，推荐使用。

paginator主要有两种类型：Paginator.requestPager和Paginator.clientPager。

Paginator.requestPager
一次请求一页的数据，下一页会继续从服务器请求数据，接口需要返回总记录数量，组件会根据配置，自动计算出总页数，此类型适用于数据量比较大的情况。



Paginator.clientPager
一次把数据全部取回来，组件会根据配置自动分页，并展示对应页码的数据，此类型适用于数据量较小的情况。





未完待续...

