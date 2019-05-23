# 如何实现数组去重？
* 假设有数组 array = [1,5,2,3,4,2,3,1,3,4]  
* 写一个函数 unique，使得  
* unique(array) 的值为 [1,5,2,3,4]  
* 也就是把重复的值都去掉，只保留不重复的值。  
要求：  
* 不要做多重循环，只能遍历一次
* 请给出两种方案，一种能在 ES 5 环境中运行，一种能在 ES 6 环境中运行（提示 ES 6 环境多了一个 Set 对象）
# 方案1
* 使用**ES5**的使用到的[indexOf()方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)返回在数组中可以找到一个给定元素的第一个索引，**如果不存在，则返回-1**。
* 代码如下：  
```
var array = [1, 5, 2, 3, 4, 2, 3, 1, 3, 4]
     function unique(a){
       var newArray=[]
       for(var i=0;i<a.length;i++){
           if(newArray.indexOf(a[i])===-1){
             newArray.push(a[i])
           }
       }
       return newArray
     }
console.log(unique(array))
//unique(array) 的值为 [1,5,2,3,4]
```
# 方案2
* 同样是使用**ES5**的使用到的[indexOf()方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)返回在**数组中可以找到一个给定元素的第一个索引**，如果不存在，则返回-1。
* 代码如下： 
```
var array = [1, 5, 2, 3, 4, 2, 3, 1, 3, 4]
     function unique(a){
       var newArray=[]
       for(var i=0;i<a.length;i++){
           if(a.indexOf(a[i])===i){
             newArray.push(a[i])
           }
       }
       return newArray
     }
console.log(unique(array))
//unique(array) 的值为 [1,5,2,3,4]
```
# 方案3
* 使用**ES6**的Set对象，[Set 对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)允许你存储任何类型的唯一值，无论是原始值或者是对象引用。
* 如果a是一个Set对象，通过[展开语法(Spread syntax)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_syntax)可以把a转换为数组.比如[...a]。
* 代码简洁很多，如下
```
var array = [1, 5, 2, 3, 4, 2, 3, 1, 3, 4]
var a=new Set(array)
console.log([...a])
//[...a]的值为 [1,5,2,3,4]
```
# 其他
## ES3的方法去重
* 使用**ES3**的使用到的[sort()方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)用原地算法对数组的元素进行排序，并返回数组。
* sort()不输入任何参数，会使得被排序的数组里面的数值按照从小到大排序，**也就是已经改变了原数组的顺序**。
* 那么有该方案有两种方式，
* 第一种方式**对比需要去重的数组经过sort()后的前一个和后一个是否相同**，如果相同就抛弃，如果不同就保留。
* 代码如下：
```
var array = [1, 5, 2, 3, 4, 2, 3, 1, 3, 4]
     function unique(a){
       a.sort()
       var newArray=[]
       for(var i=0;i<a.length;i++){
           if(a[i]!==a[i+1]){
             newArray.push(a[i])
           }
       }
       return newArray
     }
console.log(unique(array))
//unique(array) 的值为 [1,5,2,3,4]
```
* 第二种方式**新建一个数组，把经过sort()后需要去重的数组的第一个下标对应的数值作为新建数组的第一个下标的数值，然后对比这个新建数组新加入的数值与经过sort()后需要去重的数组的第一个下标的数值是否相同**，如果相同就抛弃，如果不同就保留。
* 代码如下：
```
var array = [1, 5, 2, 3, 4, 2, 3, 1, 3, 4]
     function unique(a){
        a.sort()
       var newArray=[a[0]]
       for(var i=1;i<a.length;i++){
           if(a[i]!==newArray[newArray.length-1]){
             newArray.push(a[i])
           }
       }
       return newArray
     }
console.log(unique(array))
//unique(array) 的值为 [1,5,2,3,4]
```

## 学习链接
* [ES 6 新特性汇总（一图全览）](https://zhuanlan.zhihu.com/p/24570791)  
* [ES 6 新特性列表（文字版）](https://zhuanlan.zhihu.com/p/25272098)  
* [ES 5 新增特性汇总](https://zhuanlan.zhihu.com/p/24336831)  
* [7种方法实现数组去重](https://blog.csdn.net/qappleh/article/details/80242909)
* [JS实现数组去重（重复的元素只保留一个）](https://www.cnblogs.com/jiayuexuan/p/7527055.html)
