# 代码规范说明

## 后端代码规范说明 

[google 后台开发指南](http://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/contents/)

## 前端代码规范说明

### wxml规范

#### 1. 标签位置与缩进

- 保证块元素、组件的开始符与结束符不在同一行，上下位置对齐，子元素在父元素内缩进两个空格。
```
<swiper indicator-dots="{{indicatorDots}}" autoplay="{{autoplay}}" interval="{{interval}}" duration="{{duration}}">
    <block wx:for="{{imgUrls}}" wx:key="unique">
      <swiper-item>
        <image src="{{item}}" class="slide-image" />
      </swiper-item>
    </block>
</swiper>
```
- 尽量避免多余的父标签。
- 所有wxml标签必须有结束符`</view>` `</block>` `</button>`等。

#### 2. 标签属性顺序

* `class` （class是为高而复用组件设计的，所以所以应处在第一位）
* `id`、`name`（id更加具体且应该尽量少使用，所以将它放在第二位）
* `wx:if`、`wx:else`、`wx:for`
* `wx:data`

#### 3. id/class命名规则

 - 遵循“内容优先，表现为辅”的基本原则
  -- 首先根据内容命名，如header、footer。若根据内容无法找到合适的命名，再结 合表现进行辅助，如col-main、blue-box。
 - 一律小写，多个单词以'-'连接
  -- 不能使用下划线和驼峰命名法，如main-nav。可基于最近的父元素名称作为前缀。
 - 在不影响语义的情况下，可适当使用缩写
  -- 缩写只用来表示结构，如col、nav、btn等，不可自造缩写。
 - 避免广告拦截词
  -- ad、ads、adv、banner、sponsor、gg、guangg、guanggao等，页面中尽量避免采用以上词汇来命名。

#### 4. 注释规范

页面中使用注释划分结构块。
```html
<view>
  <!-- 顶部轮播图 -->
  <swiper indicator-dots="{{indicatorDots}}" autoplay="{{autoplay}}" interval="{{interval}}" duration="{{duration}}"> ...
  </swiper>
  
  <!-- 左侧菜单 -->
  <view class="left-menu"> ...
  </view>
  
  <!-- 右侧菜单 -->
  <scroll-view class="right-menu" scroll-y="true"  bindscroll="scroll" scroll-into-view="{{toView}}" scroll-top="{{scrollTop}}" enable-back-to-top="true"> ...
  </scroll-view>

  <!-- 底部 -->
  <view class="shoppingCart-bar" wx:if="{{loading}}"> ...
  </view>
</view>

<!-- 购物车 -->
<view class="drawer-screen" bindtap="showCartList" data-statu="close" wx:if="{{showCart}}"> ...
</view>
```
----------

### wxcs规范

#### 1. 属性顺序

- 位置属性（`position`、`top`、`right`、`z-index`、`display`、`float`等）
- 大小（`width`、`height`、`margin`、`padding`等）
- 背景（`background`、`border`等）
- 文字系列（`font`、`line-height`、`letter-spacing`、`color`、`text-align`等）
- 其他（`animation`、`transition`等）
```html
.left-menu{
  position: absolute;
  left:0px;
  z-index: 10;
  width:160rpx;
  font-size:28rpx;
}
.left-menu-unselect{
  padding-left:10rpx;
  height:72rpx;
  border-bottom:1px solid #E3E3E3;
  background-color:#F9F9F9;
  color: #6C6C6C;
}
```
#### 2.属性使用缩写

在需要显示地设置所有值的情况下，应当尽量限制使用简写形式的属性声明。常见的滥用简写属性声明的情况如下：
`padding`、`margin`、`font`、`background`、`border`、`border-radius`

> 另外，对于#aabbcc形式的颜色值也可简化为#abc，这样精简代码同时又能提高用户的阅读体验。


----------


### js规范

#### 1. 语言规范

##### 1.1 变量

声明变量必须加上`var`关键字。
> 当你没有写 var, 变量就会暴露在全局上下文中, 这样很可能会和现有变量冲突. 另外, 如果没有加上, 很难明确该变量的作用域是什么。

##### 1.2 块内函数声明

不要在块内声明一个函数。
```
if(x) {
    function foo() {}
}
```
如果确实需要在块中定义函数, 建议使用函数表达式来初始化变量:
```
if(x) {
    var foo = function () {}
}
```
##### 1.3 `this`

仅在对象构造器，方法，闭包中使用。

##### 1.4 命名方式

* 变量名多个词以下划线连接
* 函数名以驼峰式命名

#### 2. 编码风格

##### 2.1 明确作用域

任何时候都要明确作用域，提高可移植性和清晰度，不要依赖于作用域链中的 window 对象。

##### 2.2 代码风格化

数组和对象的初始化，如果初始值不是很长，就保持写在单行上。
```
var arr = [ 1, 2, 3 ];  //  space after [ or before ] according to ESLint standard.
var obj = { a : 1, b : 2, c : 3 };  //  space after { or before }.
```
初始值占用多行时，缩进四个空格。
```
// Object initializer.
imgUrls: [
    '../../images/1.jpg',
    '../../images/2.jpg',
    '../../images/3.jpg',
    '../../images/4.jpg',
    '../../images/5.jpg'
    ],

```
##### 2.3 引号的使用

单引号`''`优于双引号`""`

##### 2.4 过长的单行予以换行

换行应选择在操作符和标点符号之后。
```
if (oUser.nAge < 30
    && oUser.bIsChecked === true
    || oUser.sName === 'admin') {
    // code
}
```

##### 2.5 循环的使用

在循环中，尽量使用变量先获取到循环的次数，再放入循环中进行判断，否则非常影响程序性能。
```
// 推荐
var listData = this.data.listData;
for (var i = 0; i < listData.length; i++) {
    for (var j = 0; j < listData[i].foods.length; j++) {
        listData[i].foods[j].number = 0;
    }
}
// 不推荐
for (var i = 0; i < this.data.listData.length; i++) {
    for (var j = 0; j < listData[i].foods.length; j++) {
        listData[i].foods[j].number = 0;
    }
}
```

##### 2.6 注释

###### 2.6.1 函数注释

```
/**
 * 简述
 *
 * 功能详细描述
 *
 * @param <String> arg1 参数1
 * @param <Number> arg2 参数2，默认为0
 * @return <Boolean> 判断xxx是否成功
 */
 function fooFunction (arg1, arg2) {
    // code
 }
```

###### 2.6.2 语句注释

单行注释：

- 单独一行：// 与注释文字之间保留一个空格；
- 在代码后面添加注释：//与代码之间保留一个空格，并且//与注释文字之间保留一个空格；
- //与代码之间保留一个空格。
```
// 调用了一个函数；1)单独在一行
setTitle();
var maxCount = 10; // 设置最大量；2)在代码后面注释
// setName(); // 3)注释代码
```


----------


# 设计视觉规范

#### 1. 字体

微信内字体的使用与所运行的系统字体保持一致，常用字号为20，18，17，16，14，13，11（pt），使用场景具体如下：
![此处输入图片的描述][1]


#### 2. 字体颜色

![此处输入图片的描述][2]
主内容 Black 黑色，次要内容 Grey 灰色；时间戳与表单缺省值 Light 灰色；大段的说明内容而且属于主要内容用 Semi 黑。
![此处输入图片的描述][3]
蓝色为链接用色，绿色为完成字样色，红色为出错用色 Press 与 Disable 状态分别降低透明度为20%与10%。
![此处输入图片的描述][4]

#### 3. 列表

![此处输入图片的描述][5]

#### 4. 表单输入

![此处输入图片的描述][6]

#### 5. 按钮

![此处输入图片的描述][7]
![此处输入图片的描述][8]
![此处输入图片的描述][9]
![此处输入图片的描述][10]

#### 6. 图标

![此处输入图片的描述][11]
![此处输入图片的描述][12]


  [1]: https://developers.weixin.qq.com/miniprogram/design/image/8Font.color.png
  [2]: https://developers.weixin.qq.com/miniprogram/design/image/8Font.png
  [3]: https://developers.weixin.qq.com/miniprogram/design/image/8Font.color2.png
  [4]: https://developers.weixin.qq.com/miniprogram/design/image/8Font.color3.png
  [5]: https://developers.weixin.qq.com/miniprogram/design/image/9List.png
  [6]: https://developers.weixin.qq.com/miniprogram/design/image/10Input.png
  [7]: https://developers.weixin.qq.com/miniprogram/design/image/11button.png
  [8]: https://developers.weixin.qq.com/miniprogram/design/image/11button2.png
  [9]: https://developers.weixin.qq.com/miniprogram/design/image/11button3.png
  [10]: https://developers.weixin.qq.com/miniprogram/design/image/11button4.png
  [11]: https://developers.weixin.qq.com/miniprogram/design/image/12icon.png
  [12]: https://developers.weixin.qq.com/miniprogram/design/image/13titlebar.jpg
