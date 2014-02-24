backbone.paginator-Example
==========================

backbone.paginator是backbone的分页组件，功能很稳定，推荐使用。

paginator主要有两种类型：Paginator.requestPager和Paginator.clientPager。

Paginator.requestPager
一次请求一页的数据，下一页会继续从服务器请求数据，接口需要返回总记录数量，组件会根据配置，自动计算出总页数，此类型适用于数据量比较大的情况。

现在举例说明        
首先后台提供如下接口          
接口地址：xxxUrl    
请求参数：          
xxx         //一个参数    
...       
beginRow            //从第几条记录开始取              
totleRows           //一共取多少条记录                
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

集合定义完毕，声明一个新对象。      
var xPage = new XXXPage();  

对象中重要属性：        
xPage.information   //分页的基本信息的对象，属性currentPage当前页，属性totalPages总页数，属性totalRecords总记录数    
xPage.models       //当前页的数据集合(model元素的数组)      
xPage.length       //当前页数据的数量       

对象中的重要方法：
xPage.info()        //获取information       
xPage.goTo(n)       //获取第n页的信息       
xPage.prevPage()    //上一页        
xPage.nextPage()    //下一页        
xPage.howManyPer(m) //设置每页显示的条数        

利用上面的方法可以展示出页码信息，也可以增加上翻页的操作，还可以设置改变每页的显示条数。            
获取到的model用来展示具体每一条的详细信息。         
初次展示数据，可以直接调用      
xPage.goTo(1)；         
这样子直接就获取了第一页的数据，并展示出来。        


Paginator.clientPager
一次把数据全部取回来，组件会根据配置自动分页，并展示对应页码的数据，此类型适用于数据量较小的情况。





未完待续...

