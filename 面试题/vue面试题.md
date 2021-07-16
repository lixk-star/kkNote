#### 1.vue3今年发布了，请你说一下他们之间在相应式的实现上有什么区别？

vue2采用的是defineProperty去定义get，set，而vue3改用了proxy。也代表着vue放弃了兼容ie。

https://www.cnblogs.com/cxddgz/p/13153570.html

#### 2.像vue-router，vuex他们都是作为vue插件，请说一下他们分别都是如何在vue中生效的？

通过vue的插件系统，用vue.mixin混入到全局，在每个组件的生命周期的某个阶段注入组件实例。

#### 3.请你说一下vue的设计架构

 vue2采用的是典型的混入式架构，类似于express和jquery，各部分分模块开发，再通过一个mixin去混入到最终暴露到全局的类上。

#### 4.写React / Vue 项目时为什么要在列表组件中写key，其作用是什么？

* key是给每一个vnode的唯一id,可以依靠key,更准确, 更快的拿到oldVnode中对应的vnode节点。

1. 更准确
   因为带key就不是就地复用了，在sameNode函数 a.key === b.key对比中可以避免就地复用的情况。所以会更加准确。
2. 更快
   利用key的唯一性生成map对象来获取对应节点，比遍历方式更快。

#### 5.聊聊Redux 和Vuex 的设计思想

不管是 Vue，还是 React，都需要管理状态（state），比如组件之间都有共享 
状态的需要。什么是共享状态？比如一个组件需要使用另一个组件的状态，或 
者一个组件需要改变另一个组件的状态，都是共享状态。 
父子组件之间，兄弟组件之间共享状态，往往需要写很多没有必要的代码，比 
如把状态提升到父组件里，或者给兄弟组件写一个父组件，听听就觉得挺啰嗦。 
如果不对状态进行有效的管理，状态在什么时候，由于什么原因，如何变化就 
会不受控制，就很难跟踪和测试了。如果没有经历过这方面的困扰，可以简单 
理解为会搞得很乱就对了。 
在软件开发里，有些通用的思想，比如隔离变化，约定优于配置等，隔离变化 
就是说做好抽象，把一些容易变化的地方找到共性，隔离出来，不要去影响其 
他的代码。约定优于配置就是很多东西我们不一定要写一大堆的配置，比如我 
们几个人约定，view 文件夹里只能放视图，不能放过滤器，过滤器必须放到 
filter 文件夹里，那这就是一种约定，约定好之后，我们就不用写一大堆配置文 
件了，我们要找所有的视图，直接从 view 文件夹里找就行。 
根据这些思想，对于状态管理的解决思路就是：把组件之间需要共享的状态抽 
取出来，遵循特定的约定，统一来管理，让状态的变化可以预测。根据这个思 
路，产生了很多的模式和库，我们来挨个聊聊。

#### 6.聊聊Vue 的双向数据绑定，Model 如何改变View，View 又是如何改变Model 的

* vue的数据双向绑定是通过数据劫持和发布-订阅者功能来实现的

实现步骤：1.实现一个监听者Oberver来劫持并监听所有的属性，一旦有属性发生变化就通知订阅者

2.实现一个订阅者watcher来接受属性变化的通知并执行相应的方法，从而更新视图

3.实现一个解析器compile，可以扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相对应的订阅。

#### 6.vue 单向流与双向流

https://www.cnblogs.com/xuefang-yang/p/13048510.html

看到网上很多人讨论vue是双向数据绑定，怎么又是单向数据流呢？ 其实，这两个是不同的概念，双向绑定是model改变view自动更新，view改变model自动更新；而单向数据流指的父组件传值给子组件，子组件不能修改这个值，二父组件修改这个值的话子组件也会受影响，这个影响是单向的，只能从父组件流向子组件，不能反向。

**react是单向数据绑定**

**react和vue都是单向数据流**


#### 7.虚拟Dom 的优缺点

1. 什么是虚拟dom
   用js模拟一颗dom树,放在浏览器内存中.当你要变更时,虚拟dom使用diff算法进行新旧虚拟dom的比较,将变更放到变更队列中,

   反应到实际的dom树,减少了dom操作.

   虚拟DOM将DOM树转换成一个JS对象树,diff算法逐层比较,删除,添加操作,但是,如果有多个相同的元素,可能会浪费性能,所以,react和vue-for引入key值进行区分.

优点：

- 保证性能下限： 框架的虚拟 DOM 需要适配任何上层 API 可能产生的操作，它的一些 DOM 操作的实现必须是普适的，所以它的性能并不是最优的；但是比起粗暴的 DOM 操作性能要好很多，因此框架的虚拟 DOM 至少可以保证在你不需要手动优化的情况下，依然可以提供还不错的性能，即保证性能的下限；

- 无需手动操作 DOM： 我们不再需要手动去操作 DOM，只需要写好 View-Model 的代码逻辑，框架会根据虚拟 DOM 和 数据双向绑定，帮我们以可预期的方式更新视图，极大提高我们的开发效率；

  > - 操作 DOM 慢，js运行效率高。我们可以将DOM对比操作放在JS层，提高效率。

- 跨平台： 虚拟 DOM 本质上是 JavaScript 对象,而 DOM 与平台强相关，相比之下虚拟 DOM 可以进行更方便地跨平台操作，例如服务器渲染、weex 开发等等。

  > Virtual DOM的优势不在于单次的操作，而是在大量、频繁的数据更新下，能够对视图进行合理、高效的更新。

缺点:

- 无法进行极致优化： 虽然虚拟 DOM + 合理的优化，足以应对绝大部分应用的性能需求，但在一些性能要求极高的应用中虚拟 DOM 无法进行针对性的极致优化。
  首次渲染大量DOM时，由于多了一层虚拟DOM的计算，会比innerHTML插入慢。

#### 8.$nextTick() 作用及实现原理

```javascript
//next-tick.js
/* @flow */
/* globals MutationObserver */

import { noop } from 'shared/util'
import { handleError } from './error'
import { isIE, isIOS, isNative } from './env'

export let isUsingMicroTask = false

const callbacks = []
let pending = false

function flushCallbacks () {
  pending = false
  const copies = callbacks.slice(0)
  callbacks.length = 0
  for (let i = 0; i < copies.length; i++) {
    copies[i]()
  }
}

// Here we have async deferring wrappers using microtasks.
// In 2.5 we used (macro) tasks (in combination with microtasks).
// However, it has subtle problems when state is changed right before repaint
// (e.g. #6813, out-in transitions).
// Also, using (macro) tasks in event handler would cause some weird behaviors
// that cannot be circumvented (e.g. #7109, #7153, #7546, #7834, #8109).
// So we now use microtasks everywhere, again.
// A major drawback of this tradeoff is that there are some scenarios
// where microtasks have too high a priority and fire in between supposedly
// sequential events (e.g. #4521, #6690, which have workarounds)
// or even between bubbling of the same event (#6566).
let timerFunc

// The nextTick behavior leverages the microtask queue, which can be accessed
// via either native Promise.then or MutationObserver.
// MutationObserver has wider support, however it is seriously bugged in
// UIWebView in iOS >= 9.3.3 when triggered in touch event handlers. It
// completely stops working after triggering a few times... so, if native
// Promise is available, we will use it:
/* istanbul ignore next, $flow-disable-line */
if (typeof Promise !== 'undefined' && isNative(Promise)) {
  const p = Promise.resolve()
  timerFunc = () => {
    p.then(flushCallbacks)
    // In problematic UIWebViews, Promise.then doesn't completely break, but
    // it can get stuck in a weird state where callbacks are pushed into the
    // microtask queue but the queue isn't being flushed, until the browser
    // needs to do some other work, e.g. handle a timer. Therefore we can
    // "force" the microtask queue to be flushed by adding an empty timer.
    if (isIOS) setTimeout(noop)
  }
  isUsingMicroTask = true
} else if (!isIE && typeof MutationObserver !== 'undefined' && (
  isNative(MutationObserver) ||
  // PhantomJS and iOS 7.x
  MutationObserver.toString() === '[object MutationObserverConstructor]'
)) {
  // Use MutationObserver where native Promise is not available,
  // e.g. PhantomJS, iOS7, Android 4.4
  // (#6466 MutationObserver is unreliable in IE11)
  let counter = 1
  const observer = new MutationObserver(flushCallbacks)
  const textNode = document.createTextNode(String(counter))
  observer.observe(textNode, {
    characterData: true
  })
  timerFunc = () => {
    counter = (counter + 1) % 2
    textNode.data = String(counter)
  }
  isUsingMicroTask = true
} else if (typeof setImmediate !== 'undefined' && isNative(setImmediate)) {
  // Fallback to setImmediate.
  // Technically it leverages the (macro) task queue,
  // but it is still a better choice than setTimeout.
  timerFunc = () => {
    setImmediate(flushCallbacks)
  }
} else {
  // Fallback to setTimeout.
  timerFunc = () => {
    setTimeout(flushCallbacks, 0)
  }
}

export function nextTick (cb?: Function, ctx?: Object) {
  let _resolve
  callbacks.push(() => {
    if (cb) {
      try {
        cb.call(ctx)
      } catch (e) {
        handleError(e, ctx, 'nextTick')
      }
    } else if (_resolve) {
      _resolve(ctx)
    }
  })
  if (!pending) {
    pending = true
    timerFunc()
  }
  // $flow-disable-line
  if (!cb && typeof Promise !== 'undefined') {
    return new Promise(resolve => {
      _resolve = resolve
    })
  }
}

```



#### 9.用vue的render 函数渲染三个嵌套组件？

```javascript
render: function (createElement) {
  return createElement('h1', this.blogTitle)
}
```

```javascript
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签名、组件选项对象，或者
  // resolve 了上述任何一种的一个 async 函数。必填项。
  'div',

  // {Object}
  // 一个与模板中 attribute 对应的数据对象。可选。
  {
    // (详情见下一节)
  },

  // {String | Array}
  // 子级虚拟节点 (VNodes)，由 `createElement()` 构建而成，
  // 也可以使用字符串来生成“文本虚拟节点”。可选。
  [
    '先写一些文字',
    createElement('h1', '一则头条'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
```

#### 10.给vue 的属性赋值一个对象，这个对象是否是响应的？ 给vue 新增属性是否是响应的？

1. 给属性赋值对象是响应的

2. 给vue 新增属性不是响应的

   > Vue 不允许在已经创建的实例上动态添加新的根级响应式属性 (root-level reactive property)。然而它可以使用 Vue.set(object, key, value) 方法将响应属性添加到嵌套的对象上：


#### 11.vue 首屏加载优化

1. 路由异步加载

2. 不打包库文件

   spa首屏加载慢，主要是打包后的js文件过大，阻塞加载所致。那么如何减小js的体积呢？
   那就是把库文件单独拿出来加载，不要参与打包。

3. 关闭sourcemap

   sourcemap是为了方便线上调试用的，因为线上代码都是压缩过的，导致调试极为不便，而有了sourcemap，就等于加了个索引字典，出了问题可以定位到源代码的位置。
    但是，每个js都带一个sourcemap，有时sourcemap会很大，拖累了整个项目加载速度，为了节省加载时间，我们将其关闭掉。4

4. 开启gzip 压缩

   这个优化是两方面的，前端将文件打包成.gz文件，然后通过nginx的配置，让浏览器直接解析.gz文件

5. 首页单独做服务端渲染

6. loading效果

7. 使用CDN加速

#### 12.**如何编译template 模板？**



#### 13.**React 和 Vue 的 diff 时间复杂度从 O(n^3) 优化** **到 O(n) ，那么 O(n^3) 和 O(n) 是如何计算出来的？**

1. 如果父节点不同，放弃对子节点的比较直接删除旧节点然后添加新的节点重新渲染
2. 如果子节点有变化，Virtual DOM 不会计算变化的是什么，而是重新渲染
3. 通过唯一的key 策略，对角线比较

#### 14.vue-cli 优化

### 15.在vue 中，子组件为何不可以修改父组件传递的Porp？

#### 46.keep-alive

动态组件如果不加keep-alive相当于每次都会销毁，诞生。

如果你需要在组件切换的时候，保存一些组件的状态防止多次渲染，就可以使用keep-alive组件包裹需要保存的组件。

对于keep-alive来说，她拥有两个独有的生命钩子函数，分别为activated和deactivated。用keeo-alive包裹的组件切换时不会进行销毁，而是到缓存中执行deactivated。

#### 17.组件中data为什么不用对象

* 组件复用时所有组件实例都会共享data，如果data是对象的话，就会造成一个组件修改data以后会影响其他组件，所以需要将data写成函数，每次用到就调用一次函数获得最新数据。
* 当我们使用new Vue（）的方式的时候，无论我们将data设置为对象还是函数都是可以的，因为new Vue（）的方式是生成一个根组件，改组件不会复用，也就不存在共享。

#### 18.v-show与v-if

* 实质对比：
  * v-show：通过display：none和dispaly：block之间切换。
  * v-if：通过DOM节点的插入，删除来实现。
* 性能对比：
  * v-if：切换时需要删除，插入节点，开销大。但是在初始化的时候，如果条件是false是不会插入节点，会节约性能。
    * 总结：如果不是频繁切换只需要渲染时条件用v-if
  * v-show：有更高的初始化渲染，就算为false也会渲染。但是在切换时只是改变样式，消耗少。
    * 总结：在频繁切换时用v-show

#### 19.方法调用，computed，watch的区别

* 方法：页面数据每次重新渲染都会重新执行，性能消耗大。除非不希望有缓存的时候用。
* computed：是计算属性，依赖其他属性值，并且computed的值有缓存，只有当计算值变化才会返回内容‘
* watch：监听到值的变化就会执行回调，在回调中进行一些逻辑操作。

#### 20.什么是钩子函数

* 生命周期：vue的生命周期就是vue从初始化到销毁的过程
* 钩子函数：在vue的生命周期过程中我们有很多特殊的时间段，我们希望在这些特殊时间段对vue做一些事情，作者在设置的时候定义许多在特殊时间执行的回调函数，这就是钩子函数。
* created：创建后，属性和方法挂载到的实例上面
* mounted：挂载后，vue范围内的变量都会被替换成data里面对应的数据
* beforeDestroy:销毁前，常用来清除定时器之类了。

#### 21.vue Loader

* 如果我们用脚手架开发vue的时候，肯定会接触到组件化开发，它有三种形式，全局组件，局部组件，文件组件。所谓的文件组件就是以.vue的组件形式，该文件无法被浏览器解析，vue Loader就是把.vue文件解析成浏览器能看懂了html、css、js文件
* scoped：它只作用于当前组件的元素

#### 22.什么是组件化

* 任何一个页面我们都可以抽象成由一堆组件构成一个大的组件树。一个页面就是有很多的组件嵌套拼接组件
* 组件化的好处：复用性强，分工开发，耦合度低

#### 23.介绍一下vue

* vue是有一个比较灵活的框架，也可以搭配第三方库一起使用，比如jquery，它使用的是组件化开发，提高的开发效率，而且它的指令，模板使用起来也很方便。还有一个比较高效的特点就是虚拟dom

#### 24.MVVM

* Model代表数据模型，主要任务就是操作数据

* View代表UI视图，主要任务将数据模型转化成UI视图展现出来。
* ViewModel监听模型数据的改变和控制视图行为，处理用户交互，简单来说就是一个同步View和Model的对象。连接Model和View。
  * 在MVVM架构下，View和model之间并没有直接的联系，而是通过ViewModel进行交互，Model和ViewModel之间的交互是双向的，因此View数据的变化会同步到Model中，而Model数据的变化也会立即反应到View上
  * ViewModel通过双向数据绑定把View层和Model层连接起来，而View和Model之间的同步工作完成是自动的，无需人为干涉，因此开发者只须关注业务逻辑，不需要手动操作DOM，不需要关注数据状态的同步问题，复杂的数据状态维护完全有MVVM来统一管理。