# vue-input-number
一个计数器组件，设计模仿了element的el-input-number组件，区别在于它可以更灵活地控制绑定值的变化，从而应用更复杂的业务逻辑<br/>
项目开发过程中，有需求需要使用计数器，但是需要知道用户点的是哪个按钮，以及在点击改变的过程中，需要对数值进行一些业务的逻辑处理，加入一些判断，并在列表中使用它，需要对它绑定一些参数，element中的计数器显然满足不了这个需求，所以我自己写了这个组件，也是阅读了element源码，加入了自己的一些功能实践。最终需求很好地得到解决。这个组件是不依赖于element的，所以放心使用。<br/>
## 使用方法：
* clone inputNumber.vue文件到本地，放入项目components目录下，直接当做子组件引用就行了（可以参照demo用法）
```javascript
  /**
   * 使用方法：
   * @[v-model] { number } 绑定值
   * @[min] { number } 最小值
   * @[max] { number } 最大值
   * @[disabled] { boolean } 是否禁用
   * @[step] { number } 增加或减少的步数（可以为小数，计算结果自动保留两位小数）
   * @[size] { string } 尺寸，默认（高40px）、medium（高36px）、small（高32px）、mini（高28px）
   * @[change] { function } 绑定值被改变时触发，回调参数（改变后的值,点击的按钮类型，传入的参数），按钮类型：点击加按钮值为'increase';点击减按钮值为'decrease';如果没有点击按钮，则按钮类型值为null
   * @[blur] { function } Input 失去焦点时触发，回调参数（改变后的值）
   * @[focus] { function } Input 获得焦点时触发，回调参数（改变后的值）
   * @[extendParams] {  } 传入的参数， 通过change事件返回给父组件，如果未绑定，则是undefined
   */
```