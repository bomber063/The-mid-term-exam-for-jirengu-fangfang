# 移动端是怎么做适配的？
主要有三个方面：  
## 1. 写上meta标签
主要用到meta的两个属性，name和content。 
* name属性定义一段文档级元数据的名称  
name="viewport"  
* 它提供有关视口初始大小的提示，仅供移动设备使用  
* content属性包含http-equiv 或name 属性的值的**内容**，具体取决于所使用的值。  
当name="viewport"时，对应的content值可以取下面的值：  
* width=device-width——定义视口的宽度为设备宽度;  
* user-scalable=no——如果设置为 no，用户将不能放大或缩小网页。默认值为 yes;  
* initial-scale=1.0——初始的缩放比率为1.0;  
* minimum-scale=1.0——定义最小缩放比率为1.0;  
* maximum-scale=1.0——定义最大缩放比率为1.0。  
完整的meta标签信息就是  
```
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0>"  
```
* 如果专门针对某个宽度的移动端是适配，比如某类智能手机的宽度来设计一个页面。  
当没有写上该标签的信息的时候，那么JavaScript取浏览器的宽度会按照980px的宽度来取(通过接口document.documentElement.clientWidth)，但是市面上大部分手机的宽度是要小于这个宽度的，那么整个页面就会有自动缩小的效果。  
* 已经按照某类智能手机适配好了，如果会自动缩小会影响显示效果。  
## 2.媒体查询  
* 主要通过使用@media来响应不同设备的宽度。  
* 比如果在宽度为425px以下的设备上面让整个body背景显示为红色，只需要在CSS中，或者style中写上  
```
@media (min-width:425px){
    body{
        background:red;
    }
}
```
* 使用媒体查询需要注意写在后面的媒体查询会覆盖前面的媒体查询，比如下面的代码，满足最小宽度300px，肯定就满足了最小宽度425px，并且300px写在425px的后面，那么越后面的就会覆盖前面的，这样就不会出现425px的样式了。  
```
@media (min-width:425px){
    body{
        background:red;
    }
}

@media (min-width:300px){
    body{
        background:black;
    }
}
```
* 这里可以**使用!important**规则覆盖其他声明，导致改变了优先级，有时候可以用，但是尽量不要使用该方法，具体说明见[MDN说明](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity)  
* 取而代之，你可以更好地利用CSS级联属性  
* 使用**更具体的规则**。在您选择的元素之前，**增加一个或多个其他元素**，使选择器变得更加具体，并获得更高的优先级。
* 比如下面的代码
html代码  
```
<div id="test">
  <span>Text</span>
</div>
```
css代码
```
div#test span { color: green }
span { color: red }
div span { color: blue }
```
* 无论你c​ss语句的顺序是什么样的，文本都会是绿色的(green)，因为这一条规则是最有针对性、优先级最高的。同理，无论语句顺序怎样，蓝色(blue)的规则都会覆盖红色(red)的规则
* 为了防止有宽度交集的覆盖，可以使用**逻辑操作符and**使得更好的控制宽度的边界。比如  
```
@media (min-width: 400px) and (max-width: 500px) { ... }
```
* 这样就只限定在400px和500px之间，包括了400px和500px;  
## 3.动态 rem 方案
* rem的主要思想是缩放。  
* 根据媒体查询我们可以电脑和手机的宽度来布局不同的方案来达到响应式的效果，但是单独在手机上面，就不方便通过媒体查询的方式，因为手机的品种太多导致宽度太多，这样就需要写成特别多的媒体查询。并且也会导致一定的宽窄不一。所以用动态rem方案就可以来实现移动端的适配。  
* 那么如何通过调整宽度就能够达到适应宽高比的保持一致的效果呢。vw是可以调整宽度的，但是他的兼容性比较差。  
* 动态rem就是**通过获取到移动端设备的宽度，并把该宽度作为rem的值**。这样我们使用rem不仅可以按照设备宽度调整各个元素的宽度，**还可以按照设备宽度来调整各个元素的高度**，更重要的是还可以通过**设备的宽度来调整maring和padding**，以达到适配方案。  
* CSS不能获取到移动端设备的宽度，但是JavaScript可以，通过[Window​.inner​Width](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/innerWidth)即可获取到设备的宽度，**如果存在垂直滚动条则包括它**，这里就类似于用到CSS长度单位里面[length](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length)用到的vw(视口宽度的 1/100)。  
* JavaScript代码如下就**可以获取到移动端设备的宽度**：  
```
var pageWidth=window.innerWidth
```
* JavaScript代码如下就可以把**获取到移动端设备的宽度作为css的html元素的font-size属性的值，也就是作为一个rem**：  
```
document.write('<style>html{font-size:'+pageWidth+'px;}</style>')
```
* 这样宽度变化的时候我们只需要设置不同的rem就可以调整width、height、margin、padding等各种属性了。  
* 我们还可以继续考虑把pageWidth/100，这样得到的值正好等于vw，因为vw就是视口宽度的百分之一。但是要注意，如果设备宽度的百分之一是一个**小数**，比如宽度是345，那么百分之一就是3.45,这样显示的效果就会有瑕疵，因为css最小是按照整数来计算的。会忽略掉后面的小数。并且在**chrome浏览器中可以限制字体大小**，所以要特别注意。  
* **所以把pageWidth/10是比较好的优化。**  
* 还要注意一点，就是当设置的属性的值比较小的时候，比如boder设置本来想要1px,就没有必要使用rem，可以直接写成1px，另外字体如果比较小也是可以直接用px，而不用rem。或者px和rem或者其他单位混用。  
* 附上jsbin的代码[链接](https://jsbin.com/tiroxejipu/1/edit?html,css,js,output)  
### 在 SCSS 里使用 PX2REM  
* 以上的优化还不够完善，如果能够不用计算而直接写出px是不是更棒？也就是写px自动转换为rem，那么SASS就可以做到。  
* 首先创建文件夹和文件  
```
mkdir ~/Desktop/scss-demo
cd ~/Desktop/scss-demo
mkdir scss css
touch scss/style.scss
start scss/style.scss
```
* 如果在window系统上面安装sass需要FQ。可以在git bash中使用如下代码：  
```
npm config set registry https://registry.npm.taobao.org/
touch ~/.bashrc
echo 'export SASS_BINARY_SITE="https://npm.taobao.org/mirrors/node-sass"' >> ~/.bashrc
source ~/.bashrc
npm i -g node-sass
```
* 如果在输入以下命令,可以把该目录下的scss文件转换为css目录下面的style.css文件，就通过下面的命令。
* 这里需要**写出详细的输出文件名字**，[英文官网](https://sass-lang.com/guide)和谷歌搜索sass中文后找到的[网站](https://www.sasscss.com/)上我没有找到针对**windows在node.js**下面的转换，但是在谷歌上面搜报错找到了[信息](https://github.com/armin-pfaeffle/sass-autocompile/issues/86)，在谷歌上面搜node-sass也可以找[信息](https://github.com/sass/node-sass)  
```
$ node-sass style.scss ../css/style.css
```
* 还可以写成压缩版本的  
```
$ node-sass style.scss ../css/style.css --output-style compressed
```
* 而在**ruby.sass**下面，多了一个冒号:,去掉前面的node，可以写成  
```
$ sass style.scss : ../css/style.css
```
* 如果想要每次变动scss文件都会获得css文件，那么只需要监听scss的变动即可，window的git bash代码如下:  
```
$ node-sass --watch style.scss ../css/style.css
```
* windowde的git bash还有简写的方式  
```
node-sass -wr style.scss -o ../css/style.css
```
* 这样就可以把sass实时转换成css，例如在sass中输入  
```
body{
    color:red;
    p{
        color:green;
    }
}     
```
* 就会生成一个style.css文件放在css目录里面，代码内容为：  
```
body {
  color: red; }
  body p {
    color: green; }
```
* OK，了解了运作过程我们就可以直接用了，在谷歌上搜scss px2rem，就可以找到[信息](https://github.com/imochen/luna/blob/master/root/src/px2rem.scss)  
* 把scss代码经过修改  
```
@function px2rem( $px ){
	@return $px/$designWidth*10+ rem;
}

$designWidth : 640;

.child{
    float:left;
    width:px2rem( 320 );
    background:red;
    font-size:1.2rem;
    margin:px2rem( 40 ) px2rem( 40 );
    text-align: center;
    height:px2rem( 160 );
  }
```
* 如果在监听中，就会在css中输出  
```
.child {
  float: left;
  width: 5rem;
  background: red;
  font-size: 1.2rem;
  margin: 0.625rem 0.625rem;
  text-align: center;
  height: 2.5rem; }
```
* 这样就实现了自动计算的过程，直接输入px就可以自动转换为rem。  
## 其他说明  
* 一般在文章类型(布局比较简单的网页)的会使用@media查询  
* 还有些公司比如京东和淘宝会针对**移动端专门用一个域名来写网页**，有些比如知乎或者写代码啦就会**写一套针对移动端的网页代码**。这种方式主要是由后端来控制，前端部分只需要写好前端的代码即可。  
* 百分比宽度布局的局限性是，不方便**按照高宽比来调整高度**，因为移动端一般是需要适应各种宽度，高度很少去适应，通过百分比宽度布局，局限性就是**高度**是不会按照**父元素的宽度**来改变。也就是说高度不方便与宽度做相应的配合。**宽度是不确定的**。特别是对于图片的显示就会导致长宽比例不一。  
* 也可以按照一种移动端手机的宽度来布局，并设置居中，这样如果比这种手机宽的手机只是两边会多留出一些空隙出来，但是如果比这种手机宽度小的就会导致有些地方不能一下全部看到了。  
* 当然还可以算出各个属性值与父元素的关系，因为width的百分比是与父元素的宽度有关，height的百分比是与父元素的高度有关，margin和padding是与父元素的宽度有关，但是父元素并不是页面的根元素，这样并不能与整个网页达到适配的效果，那么就会有**巨大的分析与计算量**。  
* meta学习链接可以查看[meta MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta)    
* document.documentElement.clientWidth的[MDN说明链接](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/clientWidth)，**它不包括垂直滚动条（如果有）、边框和外边距**。
* 媒体查询详细说明见[媒体查询MDN链接](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)  
#### 手机端的交互方式不一样
* 没有 hover  
* 有 touch 事件  
* 没有 resize  
* 没有滚动条  
***
2019.7.9答疑群新旺老师补充
1. viewport是指用户设备的可视区域的，需要先了解什么是设备像素，什么是CSS像素。
2. 设备像素就是指屏幕分辨率，举例IPhone6 Plus是1920*1080，它的宽有1080个像素点，但是它的CSS像素就是414px.
3. 
```
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0>"
```
4. device-witdh就是设备宽度，它和CSS像素是相同的。
5. 当我们设置width=device-width就是让我们的移动端设备的宽度为对应的CSS像素。
6. initial-scale属性控制页面最初加载时的缩放比例大小。
7. maximum-scale，minimum-scale就是最大缩放和最小缩放比例
8. CSS像素由像素设备比决定
9. 设备宽度由CSS像素决定

* 面试的时候最好以举例子为准。

* 另外一个[从PX单位到的CSS像素学习链接](https://www.jianshu.com/p/95416caf34b5)
* [CSS像素、设备独立像素、设备像素之间关系](https://www.cnblogs.com/jiangzilong/p/6700023.html)
* [MDN在移动浏览器中使用viewport元标签控制布局](https://developer.mozilla.org/zh-CN/docs/Mobile/Viewport_meta_tag)
***