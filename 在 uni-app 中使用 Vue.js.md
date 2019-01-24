`uni-app` 在发布到 App 和小程序时，借鉴 `mpvue` 的实现，修改了 `Vue.js` 的 `runtime` 和 `compiler` 实现；鉴于跨平台及 App 特殊需求，相比 Web 平台， `Vue.js` 在 `uni-app` 中使用时会存在一些差异，主要集中在两个方面：

- 新增：比如 `uni-app` 除了支持 Vue 实例的生命周期，还支持应用启动、页面显示等生命周期
- 受限：相比 web 平台，部分功能受限，比如 `v-html` 指令，具体见下。（受限部分仅在App和小程序端受限，H5版不受限）



## 生命周期
`uni-app` 完整支持 `Vue` 实例的生命周期，同时还支持 [应用生命周期](https://uniapp.dcloud.io/frame?id=应用生命周期) 及 [页面生命周期](https://uniapp.dcloud.io/frame?id=页面生命周期)。

支持 `vue` 实例的如下生命周期函数：

- beforeCreate  在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。
- created       在实例创建完成后被立即调用。然而，挂载阶段还没开始，$el 属性目前不可见。
- beforeMount   在挂载开始之前被调用：相关的 render 函数首次被调用。
- mounted       el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用。
- beforeUpdate  数据更新时调用，发生在虚拟 DOM 打补丁之前。适合在更新之前访问现有 DOM，比如手动移除已添加的事件监听器。
- updated       由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

详细的 `vue` 生命周期文档请看[生命周期钩子](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)。

**注意**
- 不要在选项属性或回调上使用箭头函数，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数是和父级上下文绑定在一起的，`this` 不会是如你做预期的 `Vue` 实例，且 `this.a` 或 `this.myMethod` 也会是未定义的。
- 建议使用 `uni-app` 的 `onReady` 代替 `vue` 的 `mounted`。
- 建议使用 `uni-app` 的 `onLoad` 代替 `vue` 的 `created`。

