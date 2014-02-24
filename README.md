backbone.paginator-Example
==========================

backbone.paginator是backbone的分页组件，功能很稳定，推荐使用。

paginator主要有两种类型：Paginator.requestPager和Paginator.clientPager。

Paginator.requestPager
一次请求一页的数据，下一页会继续从服务器请求数据，接口需要返回总记录数量，组件会根据配置，自动计算出总页数，此类型适用于数据量比较大的情况。

现在以后台接口返回json类型数据来说明
接口地址：xxxUrl
请求参数：xxx   //一个参数
          ...
          beginRow  //从第几条记录开始取  参数命名可调整
          totleRows //一共取多少条记录    参数命名可调整
接口返回：
{'totle': xxx, 'result': xxx}    //totle所有记录的总数量    result指定行的记录结果集

有了这样子的接口，分页集合可以如下方法定义：
var XXXPage = Backbone.Paginator.requestPager.extend({

    model: XXXmodel,       //集合的模型

    paginator_core: {
        url: xxxUrl,
        type: 'POST',
        headers: {
            'serviceType': 'XXX',
            'UT': XXX,
            'Content-Type': 'XXX',
            'Accept': 'XXX'
        },
        dataType: 'json',
        processData: false      //根据情况设定
    },

    paginator_ui: {          //页码配置信息
        firstPage: 1,        //第一页是哪一页
        currentPage: 1,      //当前页是第几页
        perPage: 50          //每页显示的条目数
    },

    server_api: {            //接口请求参数
        xxx: XXX,
        ...
        beginRow: function () {
            return (this.currentPage - 1) * this.perPage      //计算出开始取记录的行数
        },
        totleRows: function () {
            return this.perPage                               //需要返回多少条记录
        }
    },

    parse: function (response) {                //接口返回结果处理                  
        this.count = response.totle;            //记录的总数量                  
        this.totalRecords = response.totle;     //记录的总数量  有这个数据才可以算出总页数 
        return response.result;                 //当前页需要返回的数据
    }
});


Paginator.clientPager
一次把数据全部取回来，组件会根据配置自动分页，并展示对应页码的数据，此类型适用于数据量较小的情况。





未完待续...

