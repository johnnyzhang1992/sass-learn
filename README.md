# sass-learn

sass练习

## 1.使用变量

### 1-1. 变量声明

> 当变量定义在css 规则块内,那么该变量只能在该规则块内使用

### 1-2. 变量引用

> 在声明变量时，变量值也可以引用其他变量。

### 1-3. 变量名用中划线还是下划线分隔;

> sass的变量名可以与css中的属性名和选择器名称相同，包括中划线和下划线

> 不过，sass并不想强迫任何人一定使用中划线或下划线，所以这两种用法相互兼容。

## 2. 嵌套CSS 规则

> 一个给定的规则块，既可以像普通的CSS那样包含属性，又可以嵌套其他规则块。
>容器元素的样式规则会被单独抽离出来，而嵌套元素的样式规则会像容器元素没有包含任何属性时那样被抽离出来

### 2-1. 父选择器的标识符&;

> 一般情况下，sass在解开一个嵌套规则时就会把父选择器（#content）通过一个空格连接到子选择器的前边（article和aside）形成（#content article和#content aside）。这种在CSS里边被称为后代选择器，因为它选择ID为content的元素内所有命中选择器article和aside的元素。但在有些情况下你却不会希望sass使用这种后代选择器的方式生成这种连接。

### 2-2. 群组选择器的嵌套;

> 在CSS里边，选择器h1,h2和h3会同时命中h1元素、h2元素和h3元素。与此类似，.button ,button会命中button元素和类名为.button的元素。这种选择器称为群组选择器。群组选择器 的规则会对命中群组中任何一个选择器的元素生效。

### 2-3. 子组合选择器和同层组合选择器：>、+和~

### 2-4. 嵌套属性

## 3. 导入SASS文件

> sass的@import规则在生成css文件时就把相关文件导入进来
> 使用sass的@import规则并不需要指明被导入文件的全名

### 3-1. 使用SASS部分文件

> sass局部文件的文件名以下划线开头。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入。当你@import一个局部文件时，还可以不写文件的全名，即省略文件名开头的下划线。举例来说，你想导入themes/_night-sky.scss这个局部文件里的变量，你只需在样式表中写<code>@import "themes/night-sky"</code>;

### 3-2. 默认变量值;

> <code>!default</code>用于变量，含义是：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。

<pre>
$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
</pre>

### 3-3. 嵌套导入;

> 跟原生的css不同，sass允许@import命令写在css规则内。这种导入方式下，生成对应的css文件时，局部文件会被直接插入到css规则内导入它的地方。

### 3-4. 原生的CSS导入;

由于sass兼容原生的css，所以它也支持原生的CSS@import。尽管通常在sass中使用@import时，sass会尝试找到对应的sass文件并导入进来，但在下列三种情况下会生成原生的CSS@import，尽管这会造成浏览器解析css时的额外下载：

<ul>
<li>被导入文件的名字以.css结尾；</li>
<li>被导入文件的名字是一个URL地址（比如http://www.sass.hk/css/css.css），由此可用谷歌字体API提供的相应服务；</li>
<li>被导入文件的名字是CSS的url()值。</li>
</ul>

## 4. 静默注释;

css中注释的作用包括帮助你组织样式、以后你看自己的代码时明白为什么这样写，以及简单的样式说明。
但是，你并不希望每个浏览网站源码的人都能看到所有注释。

sass另外提供了一种不同于css标准注释格式/* ... */的注释语法，即静默注释，其内容不会出现在生成
的css文件中。静默注释的语法跟JavaScript``Java等类C的语言中单行注释的语法相同，它们以//开头，
注释内容直到行末。
<pre>
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
</pre>
实际上，css的标准注释格式/* ... */内的注释内容亦可在生成的css文件中抹去。当注释出现在原生css
不允许的地方，如在css属性或选择器中，sass将不知如何将其生成到对应css文件中的相应位置，于是
这些注释被抹掉。
<pre>
body {
  color /* 这块注释内容不会出现在生成的css中 */: #333;
  padding: 1; /* 这块注释内容也不会出现在生成的css中 */ 0;
}
</pre>
你已经掌握了sass的静默注释，了解了保持sass条理性和可读性的最基本的三个方法：
嵌套、导入和注释。现在，我们要进一步学习新特性，这样我们不但能保持条理性还能写出更好的样式。
首先要介绍的内容是：使用混合器抽象你的相关样式

## 5. 混合器;

混合器使用@mixin标识符定义。看上去很像其他的CSS @标识符，比如说@media或者@font-face。这个标识符给一大段样式赋予一个名字，这样你就可以轻易地通过引用这个名字重用这段样式。下边的这段sass代码，定义了一个非常简单的混合器，目的是添加跨浏览器的圆角边框。
<pre>
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
</pre>
然后就可以在你的样式表中通过@include来使用这个混合器，放在你希望的任何地方。@include调用会把混合器中的所有样式提取出来放在@include被调用的地方。如果像下边这样写：
<pre>
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
</pre>
//sass最终生成：
<pre>
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
</pre>

### 5-1. 何时使用混合器;

判断一组属性是否应该组合成一个混合器，一条经验法则就是你能否为这个混合器想出一个好的名字。
如果你能找到一个很好的短名字来描述这些属性修饰的样式，比如<code>rounded-corners</code>,<code>fancy-font</code>或者
<code>no-bullets</code>，那么往往能够构造一个合适的混合器。如果你找不到，这时候构造一个混合器可能并不合适。

### 5-2. 混合器中的CSS规则;

混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性。

混合器中的规则甚至可以使用sass的父选择器标识符&。使用起来跟不用混合器时一样，
sass解开嵌套规则时，用父规则中的选择器替代&

### 5-3. 给混合器传参;

混合器并不一定总得生成相同的样式。可以通过在@include混合器时给混合器传参，来定制混合器生成的精确样式。当@include混合器时，参数其实就是可以赋值给css属性值的变量。如果你写过JavaScript，这种方式跟JavaScript的function很像：
<pre>
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
</pre>
当混合器被@include时，你可以把它当作一个css函数来传参。如果你像下边这样写：
<pre>
a {
  @include link-colors(blue, red, green);
}
</pre>
//Sass最终生成的是：
<pre>
a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
</pre>
sass允许通过语法<code>$name: value</code>的形式指定每个参数的值。这种形式的传参，参数顺序就不必再在乎了，只需要保证没有漏掉参数即可：
<pre>
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
</pre>

### 5-4. 默认参数值;

为了在@include混合器时不必传入所有的参数，我们可以给参数指定一个默认值。参数默认值使用$name: default-value的声明形式，
默认值可以是任何有效的css属性值，甚至是其他参数的引用，如下代码：
<pre>
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
</pre>
如果像下边这样调用：@include link-colors(red) $hover和$visited也会被自动赋值为red。
编译后：
<pre>
.link a {  color: red; }
.link a:hover {  color: red; }
.link a:visited {  color: red; }
</pre>

## 6. 使用选择器继承来精简CSS

使用sass的时候，最后一个减少重复的主要特性就是选择器继承。基于Nicole Sullivan面向对象的css的理念，
选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。

### 6-1. 何时使用继承

当一个元素拥有的类（比如说.seriousError）表明它属于另一个类（比如说.error），这时使用继承再合适不过了。

### 6-2. 继承的高级用法
任何css规则都可以继承其他规则，几乎任何css规则也都可以被继承。大多数情况你可能只想对类使用继承，
但是有些场合你可能想做得更多。最常用的一种高级用法是继承一个html元素的样式。尽管默认的浏览器样式不会被继承，
因为它们不属于样式表中的样式，但是你对html元素添加的所有样式都会被继承。

### 6-3. 继承的工作细节

跟变量和混合器不同，继承不是仅仅用css样式替换@extend处的代码那么简单。为了不让你对生成的css感觉奇怪，
对这背后的工作原理有一定了解是非常重要的。

@extend背后最基本的想法是，如果<code>.seriousError @extend .error</code>， 那么样式表中的任何一处.error都用<code>.error``.seriousError</code>
这一选择器组进行替换。这就意味着相关样式会如预期那样应用到<code>.error和.seriousError</code>。当.error出现在复杂的选择器中，
比如说<code>h1.error``.error a</code>或者<code>#main .sidebar input.error[type="text"]</code>，那情况就变得复杂多了，但是不用担心，
sass已经为你考虑到了这些。

关于@extend有两个要点你应该知道。
<ul>
<li>
跟混合器相比，继承生成的css代码相对更少。因为继承仅仅是重复选择器，而不会重复属性，所以使用继承往往比混合器生成的css体积更小。
如果你非常关心你站点的速度，请牢记这一点。
</li>
<li>
继承遵从css层叠的规则。当两个不同的css规则应用到同一个html元素上时，并且这两个不同的css规则对同一属性的修饰存在不同的值，
css层叠规则会决定应用哪个样式。相当直观：通常权重更高的选择器胜出，如果权重相同，定义在后边的规则胜出。

</li>
</ul>
混合器本身不会引起css层叠的问题，因为混合器把样式直接放到了css规则中，而继承存在样式层叠的问题。
被继承的样式会保持原有定义位置和选择器权重不变。通常来说这并不会引起什么问题，但是知道这点总没有坏处

### 6-4. 使用继承的最佳实践

通常使用继承会让你的css美观、整洁。因为继承只会在生成css时复制选择器，而不会复制大段的css属性。
但是如果你不小心，可能会让生成的css中包含大量的选择器复制。
