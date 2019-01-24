# uni-app
uni-app 是一个使用 Vue.js 开发跨平台应用的前端框架，开发者编写一套代码，可编译到iOS、Android、H5、小程序等多个平台。

`uni-app` 借鉴整合了 `Vue.js`、`mpvue` 等前端开源框架。
同时，为保证微信小程序兼容，`uni-app` 还借鉴了微信小程序的组件规范。



## 开发规范



## 目录结构
一个uni-app工程，默认包含如下目录及文件：

    ┌─components            uni-app组件目录
    │  └─comp-a.vue         可复用的a组件
    ├─hybrid                存放本地网页的目录，[详见](https://uniapp.dcloud.io/component/web-view)
    ├─platforms             存放各平台专用页面的目录，[详见](https://uniapp.dcloud.io/platform)
    ├─pages                 业务页面文件存放的目录
    │  ├─index
    │  │  └─index.vue       index页面
    │  └─list
    │     └─list.vue        list页面
    ├─static                存放应用引用静态资源（如图片、视频等）的地方，**注意：** 静态资源只能存放于此
    ├─common                公用的资源（js、css、less/scss）
    ├─main.js               Vue初始化入口文件
    ├─App.vue               应用配置，用来配置App全局样式以及监听 [应用生命周期](https://uniapp.dcloud.io/frame?id=应用生命周期)
    ├─manifest.json         配置应用名称、appid、logo、版本等打包信息，[详见](https://uniapp.dcloud.io/collocation/manifest)
    └─pages.json            配置页面路由、导航条、选项卡等页面类信息，[详见](https://uniapp.dcloud.io/collocation/pages)

**Tips**
- `static` 目录下的 js 文件不会被编译，如果里面有 `es6` 的代码，不经过转换直接运行，在手机设备上会报错。
- `css、less/scss` 等资源同样不要放在 `static` 目录下，建议这些公用的资源放在 `common` 目录下。

|有效目录	|说明			|
|--	|--	|
|app-plus	|5+App			|
|h5			|H5				|
|mp-weixin	|微信小程序		|
|mp-alipay	|支付宝小程序     |
|mp-baidu	|百度小程序		|



## 生命周期

### 应用生命周期
`uni-app` 支持如下应用生命周期函数：

|函数名				|说明												|
|--	|--	|
|onLaunch			|当uni-app 初始化完成时触发（全局只触发一次）			|
|onShow				|当 uni-app 启动，或从后台进入前台显示					|
|onHide				|当 uni-app 从前台进入后台							|
|onUniNViewMessage	|对 nvue 页面发送的数据进行监听，可参考 nvue 向 vue 通讯 |

**注意**
应用生命周期仅可在`App.vue`中监听，在其它页面监听无效。
在应用生命周期函数内进行页面跳转时需要注意，不要和pages.json内配置的首页打开时机冲突。


### 页面生命周期
uni-app 支持如下页面生命周期函数：

|函数名      |说明           |平台支持	|
|--	|--	|--	|
|onLoad     |监听页面加载，其参数为上个页面传递的数据，参数类型为Object（用于页面传参），参考示例	|   |
|onShow     |监听页面显示         |   |
|onReady    |监听页面初次渲染完成  |   |
|onHide     |监听页面隐藏         |   |
|onUnload   |监听页面卸载         |   |
|onPullDownRefresh  |监听用户下拉动作，一般用于下拉刷新，参考示例 |   |
|onReachBottom      |页面上拉触底事件的处理函数                |   |
|onShareAppMessage  |用户点击右上角分享                       |微信小程序  |
|onPageScroll       |监听页面滚动，参数为Object               |   |
|onNavigationBarButtonTap	|监听原生标题栏按钮点击事件，参数为Object    |5+ App、H5	|
|onBackPress        |监听页面返回，详细说明及使用：onBackPress 详解  |5+App、H5	|


## 路由
`uni-app` 路由全部交给框架统一管理，开发者需要在 `pages.json` 里配置每个路由页面的路径及页面样式，不支持 `Vue Router`。


## 路由跳转
`uni-app` 有两种路由跳转方式：使用 `navigator` 组件跳转、调用 `API `跳转。


## 页面栈
框架以栈的形式管理当前所有页面， 当发生路由切换的时候，页面栈的表现如下：

|路由方式	|页面栈表现              |触发时机   |
|--	|--	|--	|
|初始化		|新页面入栈                |uni-app 打开的第一个页面   |
|打开新页面	|新页面入栈                |调用 API uni.navigateTo 、使用组件 <navigator open-type="navigateTo"/>    |
|页面重定向	|当前页面出栈，新页面入栈    |调用 API uni.redirectTo 、使用组件 <navigator open-type="redirectTo"/>    |
|页面返回	|页面不断出栈，直到目标返回页 |调用 API uni.navigateBack 、使用组件 <navigator open-type="navigateBack"/> 、用户按左上角返回按钮、安卓用户点击物理back按键	|
|Tab 切换	|页面全部出栈，只留下新的 Tab 页面	|调用 API uni.switchTab 、使用组件 <navigator open-type="switchTab"/> 、用户切换 Tab    |
|重加载		|页面全部出栈，只留下新的页面		|调用 API uni.reLaunch 、使用组件 <navigator open-type="reLaunch"/> |



## 运行环境判断
`uni-app` 可通过 `process.env.NODE_ENV` 判断当前环境是开发环境还是生产环境（在HBuilderX 中，点击运行编译出来的代码是开发环境，点击发行编译出来的代码是生产环境）。
```
if(process.env.NODE_ENV === 'development'){
    console.log('开发环境')
}else{
    console.log('生产环境')
}
```

`uni-app` 可以根据 `uni.getSystemInfoSync().platform` 判断客户端环境是 Android、iOS 还是小程序开发工具（在百度小程序开发工具、微信小程序开发工具、支付宝小程序开发工具中使用 `uni.getSystemInfoSync().platform` 返回值均为 `devtools`）。
```
switch(uni.getSystemInfoSync().platform){
    case 'android':
       console.log('运行Android上')
       break;
    case 'ios':
       console.log('运行iOS上')
       break;
    default:
       console.log('运行在开发者工具上')
       break;
}
```

**代码块**
HBuilderX 中的代码块 `uEnvDev`、`uEnvProd` 可以快速生成对应 `development`、`production` 的运行环境判定代码。
```
// uEnvDev
if (process.env.NODE_ENV === 'development') {
    // TODO
}
// uEnvProd
if (process.env.NODE_ENV === 'production') {
    // TODO
}
```

**Tips**
`uni-app` 区分当前环境是H5、App还是微信小程序，可以使用条件编译的方式，参考：条件编译



## 页面样式与布局

### 尺寸单位
`uni-app` 使用 `upx` 作为默认尺寸单位， `upx` 是相对于基准宽度的单位，可以根据屏幕宽度进行自适应。`uni-app` 规定屏幕基准宽度 `750upx`。

开发者可以通过设计稿基准宽度计算页面元素 upx 值，设计稿 1px 与框架样式 1upx 转换公式如下：
`设计稿 1px / 设计稿基准宽度 = 框架样式 1upx / 750upx`

换言之，页面元素宽度在 uni-app 中的宽度计算公式：
`750 * 元素在设计稿中的宽度 / 设计稿基准宽度`


### upx2px
动态绑定的 `style` 不支持直接使用 `upx`。
```
<!-- - 静态upx赋值生效 -->
<view class="test" style="width:200upx"></view>
<!-- - 动态绑定不生效 -->
<view class="test" :style="{width:winWidth + 'upx;'}"></view>
```

使用 uni.upx2px(Number) 转换为 px 后再赋值。
```
<template>
    <view>
        <view class="half-width" :style="{width: halfWidth}">
            半屏宽度
        </view>
    </view>
</template>

<script>
    export default {
        computed: {
            halfWidth() {
                return uni.upx2px(750 / 2) + 'px';
            }
        }
    }
</script>
<style>
    .half-width {
        background-color: #FF3333;
    }
</style>
```


### 样式导入
使用 `@import` 语句可以导入外联样式表， `@import` 后跟需要导入的外联样式表的相对路径，用 `;` 表示语句结束。

**示例代码：**
```
<style>
    @import "../../common/uni.css";

    .uni-card {
        box-shadow: none;
    }
</style>
```

### 内联样式
框架组件上支持使用 `style`、`class` 属性来控制组件的样式。

- `style`：静态的样式统一写到 `class` 中。`style` 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 `style` 中，以免影响渲染速度。
```
<view :style="{color:color}" />
```

- `class`：用于指定样式规则，其属性值是样式规则中类选择器名(样式类名)的集合，样式类名不需要带上`.`，样式类名之间用空格分隔。
```
<view class="normal_view" />
```

### 选择器
目前支持的选择器有：

|选择器				|样例			|样例描述										|
|--	|--	|--	|
|.class				|.intro			|选择所有拥有 class="intro" 的组件					|
|#id				|#firstname		|选择拥有 id="firstname" 的组件					|
|element			|view			|选择所有 view 组件								|
|element, element	|view, checkbox	|选择所有文档的 view 组件和所有的 checkbox 组件		|
|::after			|view::after	|在 view 组件后边插入内容，仅微信小程序和5+App生效	|
|::before			|view::before	|在 view 组件前边插入内容，仅微信小程序和5+App生效	|

### 全局样式与局部样式
定义在 App.vue 中的样式为全局样式，作用于每一个页面。在 pages 目录下 的 vue 文件中定义的样式为局部样式，只作用在对应的页面，并会覆盖 App.vue 中相同的选择器。

**注意**： App.vue 中通过 `@import` 语句可以导入外联样式，一样作用于每一个页面。

### CSS变量
uni-app 提供内置 CSS 变量

|CSS变量			|描述					    |5+App			|小程序	|H5						|
|--	|--	|--	|--	|
|--status-bar-height|系统状态栏高度			|系统状态栏高度	|25px	|0						|
|--window-top		|内容区域距离顶部的距离	|0				|0		|NavigationBar 的高度	|
|--window-bottom	|内容区域距离底部的距离	|0				|0		|TabBar 的高度			|

示例：
`var(--status-bar-height)` ，表示状态栏的高度。
当设置 `"navigationStyle":"custom"` 后可以使用固定一个高度为 `var(--status-bar-height)` 的 `view` 放在页面顶部，使得页面内容不会出现在状态栏里。

注意：`var(--status-bar-height)` 此变量在微信小程序环境为固定 25px，在 5+App 里为手机实际状态栏高度。
```
<template>
    <view class="index">
        <view class="status_bar">
            <view class="top_view"></view>
        </view>
        <view class="content"> Hello </view>
    </view>
</template>
<style>
    .index {
        flex: 1;
        flex-direction: column;
    }
    .status_bar {
        height: var(--status-bar-height);
        width: 100%;
    }
    .top_view{
        height: var(--status-bar-height);
        width: 100%;
        position: fixed;
        top: 0;
    }
    .content{
        justify-content: center;
        align-items: center;
    }
</style>
```

### 固定值
`uni-app` 中以下组件的高度是固定的，不可修改：

|组件			|描述		|5+App	|H5		|
|--	|--	|--	|--	|
|NavigationBar	|导航栏		|44px	|44px	|
|TabBar			|底部选项卡	|56px	|50px	|


### Flex布局
为支持跨平台，框架建议使用Flex布局，关于Flex布局可以参考外部文档[A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)等。


### 背景图片
`uni-app` 支持使用在 `css` 里设置背景图片，使用方式与普通 `web` 项目相同，需要注意以下几点：

* 支持 base64 格式图片。
* 支持网络路径图片。
* 使用本地路径图片需注意：
    1. 图片小于 40kb，uni-app 会自动将其转化为 base64 格式；
    2. 图片大于等于 40kb， 需开发者自己将其转换为base64格式使用或将其挪到服务器上，从网络地址引用。
    3. 本地图片的引用路径仅支持以 ~@ 开头的绝对路径（不支持相对路径）。
```
    .test2 {
         background-image: url('~@/static/logo.png');
     }
```


### 字体图标
`uni-app` 支持使用字体图标，使用方式与普通 `web` 项目相同，需要注意以下几点：

- 支持 base64 格式字体图标。
- 支持网络路径字体图标。
- 网络路径必须加协议头 https。
- 从 http://www.iconfont.cn 上拷贝的代码，默认是没加协议头的。
- uni-app 本地路径图标字体支持情况：
    1. 字体文件小于 40kb，uni-app 会自动将其转化为 base64 格式；
    2. 字体文件大于等于 40kb， 需开发者自己转换，否则使用将不生效；
    3. 字体文件的引用路径仅支持以 ~@ 开头的绝对路径（不支持相对路径）。
``` 
    @font-face {
        font-family: test1-icon;
        src: url('~@/static/iconfont.ttf');
    }
```
**示例：**
```
<template>
    <view>
        <view>
            <text class="test">&#xe600;</text>
            <text class="test">&#xe687;</text>
            <text class="test">&#xe60b;</text>
        </view>
    </view>
</template>
<style>
    @font-face {
        font-family: 'iconfont';
        src: url('https://at.alicdn.com/t/font_865816_17gjspmmrkti.ttf') format('truetype');
    }
    .test {
        font-family: iconfont;
        margin-left: 20upx;
    }
</style>
```