# call、apply、bind 的用法分别是什么？
## call的用法
* call就是第一个参数是可以改变this，第二个参数就是函数的参数，以**参数列表**的形式传入。
* 例如下面的代码：
```
var names1={
    name: "bomber1",
    output: function(x,y){
        console.log(this.name+x+y);
    }
}
var names2={
    name: "bomber2"
}
names1.output.call(names2,'参数1','参数2');//这里names2就是this,'参数1'和'参数2'就是x和y
//最后结果为bomber2参数1参数2
```
## apply的用法
* call就是第一个参数是可以改变this，第二个参数就是函数的参数，以**数组或者伪数组**的形式传入。
* 数组形式  
```
var names1={
    name: "bomber1",
    output: function(x,y){
        console.log(this.name+x+y);
    }
}
var names2={
    name: "bomber2"
}
names1.output.apply(names2,['参数1','参数2']);//这里names2就是this,数组['参数1'和'参数2']里面的'参数1'和'参数2'就是x和y
//最后结果为bomber2参数1参数2
```
* 伪数组形式
```
var names1={
    name: "bomber1",
    output: function(x,y){
        console.log(this.name+x+y);
    }
}
var names2={
    name: "bomber2"
}
names1.output.apply(names2,{0:'参数1',1:'参数2',length:2});//这里names2就是this,伪数组{0:'参数1',1:'参数2',length:2}里面的'参数1'和'参数2'就是x和y
//最后结果为bomber2参数1参数2
```
## bind的用法
* bind就是传入一个参数是可以改变this，并且得到一个函数。但是不会立即调用。可以赋值给一个变量。那么该变量就是一个函数。
* 再次调用该函数就可以输出绑定的this的结果，**但是此时call或者apply已经无法改变原来bind返回的this**。
```
var names1={
    name: "bomber1",
    output: function(x,y){
        console.log(this.name+x+y);
    }
}
var names2={
    name: "bomber2"
}
var a=names1.output.bind(names2)//这里的names2就是this
a.call(names1,'参数1','参数2')//这里通过call调用把this修改为nams1已经没有用了.
//最后的结果是bomber2参数1参数2
```

## 其他
* MDN上对于call的详细解释[链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
* MDN上对于apply的详细解释[链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
* MDN上对于bind的详细解释[链接](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Polyfill)
* 方方对于this的说明文章[链接](https://zhuanlan.zhihu.com/p/23804247)