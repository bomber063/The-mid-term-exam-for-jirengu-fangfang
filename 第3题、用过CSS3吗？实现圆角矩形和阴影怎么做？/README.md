# 用过CSS3吗? 
用过CSS3，它包括animation，圆角，渐变，阴影等新的属性，还有一些新的布局方式。具体见[MDN链接](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS3)  

# 实现圆角矩形和阴影怎么做?
实现圆角可以使用[border-radius属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-radius)，
比如下面代码就是圆角的半径为20px。  
```
  border-radius: 20px;
```
# 实现阴影怎么做?
阴影可以用[box-shadow属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)。
比如下面代码就是给class为shadow的类增加一个阴影  
```
.shadow{
box-shadow:10px 10px 10px 10px red;
/* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影颜色 */
}
```
# 其他
* 以上的部分CSS3效果，比如shadow(阴影)可以通过[box-shadow generator](https://www.cssmatic.com/box-shadow)，transitions(过渡)可以通过[Ultimate CSS Gradient Generator](https://www.colorzilla.com/gradient-editor/), animation(动画)可以通过[cssanimate](http://cssanimate.com/)输入相应的值来直接生成相应的CSS代码。  

* 另外我还写过一篇关于阴影的[博客](https://zhuanlan.zhihu.com/p/55900037)  
* 有一篇涉及到transitions(过渡)和animation(动画)的[博客](https://zhuanlan.zhihu.com/p/55093002)  
