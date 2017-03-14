# 让代码给自己说话 － javascript {#-javascript}

## 结构化 {#-}

### 提取函数 {#-}

代码的意义没有被表达，如下：

```
//emToPixels
var width = (value - 0.5) * 16;

```

给它起个好名字，一目了然

```
var width = emToPixels(value);

function emToPixels(ems) {
    return (ems - 0.5) * 16;
}

```

### 表达式替换为函数 {#-}

普通的写法：

```
//isVisible
if(!el.offsetWidth || !el.offsetHeight) {
    ...
}

```

看起来更有意义了

```
if(!isVisible(el)) {
    ...
}

function isVisible(el) {
    return el.offsetWidth 
&
&
 el.offsetHeight;
}

```

通过给变量名

```
//isVisible
return a * b + (c / d);

```

分开写，更加容易让人理解

```
var multiplier = a * b;
var divisor = c / d;
return multiplier + divisor;

```

### Class 和 module 接口 {#class-module-}

普通的写法：

```
class Box {
    setState(state) {
        this.state = state;
    }

    getState() {
        return this.state;
    }
}

```

推荐的写法

```
class Box {
    open() {
        this.state = 'open';
    }

    close() {
        this.state = 'closed';
    }

    isOpen() {
        return this.state === 'open';
    }
}

```

### 编写纯函数 {#-}

上面都是纯函数，就不举例了

> 没有"副作用"（side effect）： 指的是函数内部与外部互动（最典型的情况，就是修改全局变量的值），产生运算以外的其他结果。 函数式编程强调没有"副作用"，意味着函数要保持独立，所有功能就是返回一个新的值，没有其他行为，尤其是不得修改外部变量的值。
>
> 不修改状态： 上一点已经提到，函数式编程只是返回新的值，不修改系统变量。因此，不修改变量，也是它的一个重要特点。
>
> [更多可以看这里](http://www.ruanyifeng.com/blog/2012/04/functional_programming.html)

### 目录结构定义 {#-}

#### 子页面 {#-}

> 纸质弹窗、步骤条中的每个step以及业务上独立的页面需要写入单独的子页面文件夹（包括js,bs,css,html，如果有都要独立），如果子页面中还包含子页面，则在子页面的文件夹中再嵌套其子页面的文件夹。

#### 公共页面 {#-}

> 如果某个子页面在多个模块中都有用到，则需要将其放到public/commonpage/文件夹下。

#### 不同角色功能模块的拆分 {#-}

> 不同的功能模块，应写在不同的一级模块中，对于同一功能模块下的不同角色的页面，应当根据角色拆分成不同的一级模块。

## 命名 {#-}

* 建议的命名方式：变量＝小写 用 \_ 分割；函数＝驼峰 动词＋名词；提高辨识度
  ```
  var data_rows = getRows();

  ```
* 私有变量／函数 加 前缀
  _;全局变量使用全大写_
  分割

> 不要使用模糊的词语，比如：handleLinks\(\), manageObjects\(\).

```
const _TAG_PARAM = {
    "fe-req-form": _getReqControls,
    "fe-res-table": _getResControls,
    "fe-res-form": _getResControls
}

/***
 * 请求参数转换
 */
function _getReqControls(element) {
    ...
    return controls;
}

```

* 模块命名 使用模块的拼音首字母
* html元素钩子使用 data-action= ，data-action的值为中文汉字，对动作进行说明
  ```
  <
  a href="javascript:void(0)" class="bh-btn bh-btn-primary" id="addKind"
  >
  新增种类
  <
  /a
  >
  <
  script
  >

  $("#addKind").click(function() {
  });

  <
  /script
  >
  ```

  > eventMap 中 data-action 对应的方法命名应该以action开头，eventMap中由于data-action为中文，所以不需要再添加额外的注释
  >
  > ```
  > <
  > button data-action="新增种类"
  > >
  > 新增种类
  > <
  > /button
  > >
  > <
  > script
  > >
  >
  > this.eventMap = {
  >   '[data-action="新增种类"]': this.actionAddKind
  > };
  >
  > <
  > /script
  > >
  > ```
* css的命名应当添加模块名的缩写作为前缀，以免在css合并打包后，不同模块间的css相互影响

## 语法 {#-}

### 使用语言的高级特性 {#-}

一个好的栗子🌰：

```
var ids = [];
for(var i = 0; i 
<
 things.length; i++) {
  ids.push(things[i].id);
}

```

下面的方法是等价的，并且少定义几个名字哦

```
var ids = things.map(function(thing) {
  return thing.id;
});

```

### 适当情况下，减少使用语法技巧 {#-}

```
imTricky 
&
&
 doMagic();

```

这样看起来负担小一些

```
if(imTricky) {
    doMagic();
}

```

  


