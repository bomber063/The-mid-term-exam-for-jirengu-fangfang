# 什么是闭包
「函数」和「函数内部能访问到的变量」（函数内部能访问到的所有变量也叫词法环境）的总和，就是一个闭包。

# 闭包的作用
闭包常常用来「间接访问一个变量」。换句话说，「隐藏一个变量」。

**闭包就是声明一个不能被全局变量覆盖，并且别人访问不到，但是可以通过访问器（函数）访问的方式**。
## 举几个例子：
比如下面的代码
* window.奖励一条命对应的匿名函数和var lives=50构成一个闭包，window.死一条命对应的匿名函数和var lives=50构成另一个闭包，这两个闭包的作用可以看出来不同，一个是+1一个是-1。
```
!function(){
  var lives = 50
  window.奖励一条命 = function(){
    lives += 1
    return lives
  }
  window.死一条命 = function(){
    lives -= 1
    return lives
  }
}()
var lives=10//就算这里设置了lives和闭包的变量名字一样也不会覆盖。
console.log(window.死一条命())//通过访问器就可以访问到
console.log(lives)//无法访问闭包里面的lives
```
* 没有return的闭包，不需要返回信息使用，只需要做一些操作即可。
* 一个函数init里面有一个变量name，并且还有一个函数displayName，函数displayName里面没有任何变量。也没有任何返回值。    
* 这个变量name和函数displayName组成了闭包。

```
function init() {
    var name = "Mozilla"; // name 是一个被 init 创建的局部变量
    function displayName() { // displayName() 是内部函数,一个闭包
        alert(name); // 使用了父函数中声明的变量
    }
    displayName();
}
init();//外部只需要调用该init函数就可以执行相应的代码啦
```
* 有return的闭包，看return是什么，在下面的代码中return是一个函数，所以需要再次调用这个函数才可以执行相应的代码，一个函数init里面有一个变量name，并且还有一个函数displayName，函数displayName里面没有任何变量。此时返回了函数 displayName。  
* 这个变量name和函数displayName组成了闭包。

```
function makeFunc() {
    var name = "Mozilla";
    function displayName() {
        alert(name);
    }
    return displayName;
}

var myFunc = makeFunc();
myFunc();//因为返回了是一个displayName函数，所以需要再次调用该函数。
```
* 在下面的代码中makeAdder函数返回了一个匿名带参数y的函数，匿名函数继续返回x+y这两个参数相加，所以需要再次调用这个函数就可以执行相应的代码。  
* 这个参数x和参数y作为外部变量（也就是词法环境），这个匿名函数和两个参数（x和y）组成了闭包。
add5 和 add10 都是闭包。它们**共享相同的函数定义，但是保存了不同的词法环境**。在 add5 的环境中，x 为 5。而在 add10 中，x 则为 10。
```
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12
```

## 性能问题
* 如果不是某些特定任务需要使用闭包，在其它函数中创建函数是不明智的，**因为闭包在处理速度和内存消耗方面对脚本性能具有负面影响。**

* 例如，在创建新的对象或者类时，方法通常应该关联于对象的原型，而不是定义到对象的构造器中。原因是这将导致每次构造器被调用时，方法都会被重新赋值一次（也就是，每个对象的创建）。

## 其他信息
* 方方老师的闭包说明[链接](https://zhuanlan.zhihu.com/p/22486908)

* MDN关于闭包说明的[链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)

* 知乎关于闭包的说明[链接](https://www.zhihu.com/question/34210214)