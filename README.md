# Async-map

### 1.each与map的区别

##### each: 如果想对同一个集合中的所有元素都执行同一个异步操作。(同步)	

##### map:对集合中的每一个元素，执行某个异步操作，得到结果。所有的结果将汇总到最终的callback里。



#### 区别:  each只关心操作不管最后的值，而map关心的最后产生的值。



##### 下面介绍下map~

```
map （coll，iteratee，callback opt）
```

`coll`通过`iteratee`函数映射每个值来生成新的值集合。在`iteratee`被称为从一个项目`coll` ，当它已经完成处理回调。这些回调中的每一个都有两个参数：an `error`和已转换的项`coll`。如果 `iteratee`将错误传递给其回调，则主`callback`（对于 `map`函数）将立即被调用，并显示错误。

请注意，由于此功能`iteratee`并行应用于每个项目，因此不能保证`iteratee`功能将按顺序完成。但是，结果数组将与原来的顺序相同`coll`。

如果`map`传递一个Object，结果将是一个Array。结果将大致按照原始对象的键的顺序（但是JavaScript可能会有所不同）。



##### 其中包含3个参数,分别是coll ,iteratee,callback

coll: 数组 | 可替代 | 目的   要迭代的集合.

interatee: 异步功能  应用于每个项目的异步功能coll.迭代应该用转换的项目完成.用(项目,回调)调用.

callback:功能  所有iteratee 函数完成后调用的回调, 或发生错误.结果是一个转换项目的数组coll.用(error,results)调用.



## 根据课堂代码演示

```
var async = require('async');

var class1217 = [{name: '文冠龙', age: 999,delay:500},
    {name: '李青', age: 998,delay:200},
    {name: '刁刁', age: 997,delay:1000},
    {name: '班长', age: 996,delay:100},
    {name: '于谦益', age: 995,delay:1000}];


// each 并行

async.map(class1217,function (item, callback) {
        setTimeout(function () {
            var date=new Date();
            var time=date.getTime();
            // if (item.age===999){
            //     callback('此人有毒');
            // }else{
                console.log('map 1 :'+ item.name+time);
                callback(null,item);
            // }

        },item.delay);
},function done(error) {
    //如果有错误直接跳出错误
    //如果没有错误 最后执行 null.
    console.log(error);
});
```



## console.log

```
map 1 :班长1493725484388
map 1 :李青1493725484485
map 1 :文冠龙1493725484784
map 1 :刁刁1493725485284
map 1 :于谦益1493725485284
null
```



如果我们在中间插入error

```
/**
 * Created by dllo on 17/5/2.
 */

var async = require('async');

var class1217 = [{name: '文冠龙', age: 999,delay:500},
    {name: '李青', age: 998,delay:200},
    {name: '刁刁', age: 997,delay:1000},
    {name: '班长', age: 996,delay:100},
    {name: '于谦益', age: 995,delay:1000}];


// each 并行

async.map(class1217,function (item, callback) {
        setTimeout(function () {
            var date=new Date();
            var time=date.getTime();
            if (item.age===999){
                callback('此人有毒');
            }else{
                console.log('map 1 :'+ item.name+time);
                callback(null,item);
            }

        },item.delay);
},function done(error) {
    //如果有错误直接跳出错误
    //如果没有错误 最后执行 null.
    console.log(error);
});
```

## console.log

```
map 1 :班长1493725419740
map 1 :李青1493725419839
此人有毒
map 1 :刁刁1493725420639
map 1 :于谦益1493725420640

```

# 总结

map 可以输出结果 又能输出 过程.

而each 是不输出结果的,值输出过程的.

如果要进行某些操作或者计算 你想要返回的结果那你就要用map.

each有的功能map 都有 所以以后可以考虑用map.