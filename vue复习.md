# 学习使用 vue

## one

### vue 基本结构

vue 实例控制 html 元素区域，就是我们 new 出来的 vue 实例，当我们导入包之后，在浏览器的内存中，就多了一个 Vue 构造函数 el 表示要控制哪个区域

我们 new 出来的这个 vm 对象，就是我们 MVVM 中的 VM 调度者，data 中的数据可以通过 vue 指令渲染到页面中，程序员不再手动操作 DOM 元素了【前端的 Vue 之类的框架，不提倡我们去手动操作 DOM 元素了】

### 往页面显示 data 中的数据和执行函数

使用 v-cloak 能够解决 插值表达式闪烁的问题，默认 v-text 是没有闪烁问题的，v-text 会覆盖元素中原本的内容，但是 插值表达式 只会替换自己的这个占位符，不会把 整个元素的内容清空，v-bind 是 Vue 中，提供的用于绑定属性的指令，注意： `v-bind:` 指令可以被简写为 `:要绑定的属性`，v-bind 中，可以写合法的 JS 表达式，Vue 中提供了`v-on: 事件绑定机制`,也可简写`@事件绑定机制`

1. 如何定义一个基本的 Vue 代码结构
2. 插值表达式 和 v-text
3. v-cloak
4. v-html
5. v-bind Vue 提供的属性绑定机制 缩写是 :
6. v-on Vue 提供的事件绑定机制 缩写是 @

### 实现一个滚动字幕的效果

思路：在定时器中，获取字符串的第一个字符，和后面的所有字符，将第一个字符放到最后，不断循环此操作，达到字幕的滚动，定义两个函数控制开启与停止，利用是否清除定时器

```javascript
 methods: {
          lang() {
            if (this.intervalid != null) return;

            this.intervalid = setInterval(() => {
              var start = this.msg.substring(0, 1);
              var end = this.msg.substring(1);
              this.msg = end + start;// 在data上定义 定时器Id
            }, 50);
          },
          stop() {
            clearInterval(this.intervalid);
            this.intervalid = null;
          }
        }
```

### 事件修饰符

```html
<div id="app">
  <!-- 使用  .stop  阻止冒泡 -->
  <div class="inner" @click="div1Handler">
    <input type="button" value="戳他" @click.stop="btnHandler" />
  </div>
  <!-- 使用 .prevent 阻止默认行为 -->
  <a href="http://www.baidu.com" @click.prevent="linkClick">有问题，先去百度</a>

  <!-- 使用  .capture 实现捕获触发事件的机制 -->
  <div class="inner" @click.capture="div1Handler">
    <input type="button" value="戳他" @click="btnHandler" />
  </div>

  <!-- 使用 .self 实现只有点击当前元素时候，才会触发事件处理函数 -->
  <div class="inner" @click.self="div1Handler">
    <input type="button" value="戳他" @click="btnHandler" />
  </div>

  <!-- 使用 .once 只触发一次事件处理函数 -->
  <a href="http://www.baidu.com" @click.prevent.once="linkClick"
    >有问题，先去百度</a
  >

  <!-- 演示： .stop 和 .self 的区别 -->
  <div class="outer" @click="div2Handler">
    <div class="inner" @click="div1Handler">
      <input type="button" value="戳他" @click.stop="btnHandler" />
    </div>
  </div>

  <!-- .self 只会阻止自己身上冒泡行为的触发，并不会真正阻止 冒泡的行为 -->
  <div class="outer" @click="div2Handler">
    <div class="inner" @click.self="div1Handler">
      <input type="button" value="戳他" @click="btnHandler" />
    </div>
  </div>
</div>
```

### 双向数据绑定

v-bind 只能实现数据的单向绑定，从 M 自动绑定到 V， 无法实现数据的双向绑定

使用 v-model 指令，可以实现 表单元素和 Model 中数据的双向数据绑定

注意： v-model 只能运用在 表单元素中

```html
<h4>{{ msg }}</h4>
<input type="text" style="width: 100%;" v-model="msg" />
```

### 做一个简单的计算器

```javascript
 methods: {
          calc() {
            switch (this.opt) {
              case "+":
                this.result = parseInt(this.n1) + parseInt(this.n2);
                break;
              case "-":
                this.result = parseInt(this.n1) - parseInt(this.n2);
                break;
              case "*":
                this.result = parseInt(this.n1) * parseInt(this.n2);
                break;
              case "/":
                this.result = parseInt(this.n1) / parseInt(this.n2);
                break;
            }
          }
        }
```

### vue 中的 class

```html
<div id="app">
  <h1 class="red thin">这是一个很大很大的H1，大到你无法想象！！！</h1>

  <!-- 第一种使用方式，直接传递一个数组，注意： 这里的 class 需要使用  v-bind 做数据绑定 -->
  <h1 :class="['thin', 'italic']">
    这是一个很大很大的H1，大到你无法想象！！！
  </h1>

  <!-- 在数组中使用三元表达式 -->
  <h1 :class="['thin', 'italic', flag?'active':'']">
    这是一个很大很大的H1，大到你无法想象！！！
  </h1>

  <!-- 在数组中使用 对象来代替三元表达式，提高代码的可读性 -->
  <h1 :class="['thin', 'italic', {'active':flag} ]">
    这是一个很大很大的H1，大到你无法想象！！！
  </h1>

  <!-- 在为 class 使用 v-bind 绑定 对象的时候，对象的属性是类名，由于 对象的属性可带引号，也可不带引号，所以 这里我没写引号；  属性的值 是一个标识符 -->
  <h1 :class="classObj">这是一个很大很大的H1，大到你无法想象！！！</h1>
</div>
```

### ### vue 中的 style

```html
<div id="app">
  <h1 :style="styleObj1">这是一个h1</h1>

  <h1 :style="[ styleObj1, styleObj2 ]">这是一个h1</h1>
</div>
```

### vue 中的循环

```html
<p v-for="(item, i) in list">索引值：{{i}} --- 每一项：{{item}}</p>
<p v-for="(user, i) in list">
  Id：{{ user.id }} --- 名字：{{ user.name }} --- 索引：{{i}}
</p>
<p v-for="(val, key, i) in user">
  值是： {{ val }} --- 键是： {{key}} -- 索引： {{i}}
</p>
<p v-for="count in 10">这是第 {{ count }} 次循环</p>
<p v-for="item in list" :key="item.id">
  <input type="checkbox" />{{item.id}} --- {{item.name}}
</p>
```

注意：在遍历对象身上的键值对的时候， 除了 有 val key ,在第三个位置还有 一个 索引

in 后面我们可以放 普通数组，对象数组，对象， 还可以放数字

注意：如果使用 v-for 迭代数字的话，前面的 count 值从 1 开始

注意： v-for 循环的时候，key 属性只能使用 number 获取 string

注意： key 在使用的时候，必须使用 v-bind 属性绑定的形式，指定 key 的值
在组件中，使用 v-for 循环的时候，或者在一些特殊情况中，如果 v-for 有问题，必须 在使用 v-for 的同时，指定 唯一的 字符串/数字 类型 :key 值

### vue 中的判断

v-if 的特点：每次都会重新删除或创建元素

v-show 的特点： 每次不会重新进行 DOM 的删除和创建操作，只是切换了元素的 display:none 样式

v-if 有较高的切换性能消耗

v-show 有较高的初始渲染消耗

如果元素涉及到频繁的切换，最好不要使用 v-if, 而是推荐使用 v-show

如果元素可能永远也不会被显示出来被用户看到，则推荐使用 v-if

```html
<input type="button" value="toggle" @click="flag=!flag" />
<h3 v-if="flag">由v-if控制</h3>
<h3 v-show="!flag">用v-show控制</h3>
```

### 小结

1. MVC 和 MVVM 的区别

2. 学习了 Vue 中最基本代码的结构

3. 插值表达式 v-cloak v-text v-html v-bind（缩写是:） v-on（缩写是@） v-model v-for v-if v-show

4. 事件修饰符 ： .stop .prevent .capture .self .once

5. el 指定要控制的区域 data 是个对象，指定了控制的区域内要用到的数据 methods 虽然带个 s 后缀，但是是个对象，这里可以自定义了方法

6. 在 VM 实例中，如果要访问 data 上的数据，或者要访问 methods 中的方法， 必须带 this

7. 在 v-for 要会使用 key 属性 （只接受 string / number）

8. v-model 只能应用于表单元素

9. 在 vue 中绑定样式两种方式 v-bind:class v-bind:style

## two

### 品牌管理案例

分析：

_在 Vue 中，使用事件绑定机制，为元素指定处理函数的时候，如果加了小括号，就可以给函数传参了_
涉及到的操作有增删查

#### 增加

分析：

1. 获取到 id 和 name ,直接从 data 上面获取
2. 组织出一个对象
3. 把这个对象，调用 数组的 相关方法，添加到 当前 data 上的 list 中
4. 注意：在 Vue 中，已经实现了数据的双向绑定，每当我们修改了 data 中的数据，Vue 会默认监听到数据的改动，自动把最新的数据，应用到页面上；

#### 删除

分析：

1. 如何根据 Id，找到要删除这一项的索引
2. 如果找到索引了，直接调用 数组的 splice 方法

#### 查找

分析

1. 根据关键字，进行数据的搜索
2. ES6 的 filter 方法都会对数组中的每一项，进行遍历，执行相关的操作
3. ES6 中，为字符串提供了一个新方法，叫做 String.prototype.includes('要包含的字符串')，根据这个返回对应的元素

### 过滤器

过滤器中的 function ，第一个参数，已经被规定死了，永远都是 过滤器 管道符前面 传递过来的数据

`<p>{{ msg | msgFormat('疯狂', '123') | test }}</p>`

```javascript
Vue.filter("msgFormat", function(msg, arg, arg2) {
  return msg.replace(/单纯/g, arg + arg2);
});

Vue.filter("test", function(msg) {
  return msg + "========";
});
```

定义一个 Vue 全局的过滤器，名字叫做 msgFormat，字符串的 replace 方法，第一个参数，除了可写一个 字符串之外，还可以定义一个正则

全局的过滤器， 进行时间的格式化,所谓的全局过滤器，就是所有的 VM 实例都共享的

```javascript
Vue.filter("dateFormat", function(dateStr, pattern = "") {
  // 根据给定的时间字符串，得到特定的时间
  var dt = new Date(dateStr);

  //   yyyy-mm-dd
  var y = dt.getFullYear();
  var m = dt.getMonth() + 1;
  var d = dt.getDate();
  if (pattern.toLowerCase() === "yyyy-mm-dd") {
    return `${y}-${m}-${d}`;
  } else {
    var hh = dt.getHours();
    var mm = dt.getMinutes();
    var ss = dt.getSeconds();

    return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;
  }
});
```

定义私有过滤器 : 在 filters 中定义，过滤器有两个 条件 【过滤器名称 和 处理函数】,过滤器调用的时候，采用的是就近原则，如果私有过滤器和全局过滤器名称一致了，这时候 优先调用私有过滤器

#### 自定义全局指令

使用 Vue.directive() 定义全局的指令 v-focus

其中：参数 1 ： 指令的名称，注意，在定义的时候，指令的名称前面，不需要加 v- 前缀, 但是： 在调用的时候，必须 在指令名称前 加上 v- 前缀来进行调用
参数 2： 是一个对象，这个对象身上，有一些指令相关的函数，这些函数可以在特定的阶段，执行相关的操作

```javascript
Vue.directive("focus", {
  bind: function(el) {},
  inserted: function(el) {
    el.focus();
    // 和JS行为有关的操作，最好在 inserted 中去执行，放置 JS行为不生效
  },
  updated: function(el) {}
});
```

bind: 每当指令绑定到元素上的时候，会立即执行这个 bind 函数，只执行一次在每个 函数中，第一个参数，永远是 el ，表示 被绑定了指令的那个元素，这个 el 参数，是一个原生的 JS 对象在元素, 刚绑定了指令的时候，还没有 插入到 DOM 中去，这时候，调用 focus 方法没有作用 因为，一个元素，只有插入 DOM 之后，才能获取焦点

inserted: 表示元素 插入到 DOM 中的时候，会执行 inserted 函数【触发 1 次】

updated: 当 VNode 更新的时候，会执行 updated， 可能会触发多次

样式：只要通过指令绑定给了元素，不管这个元素有没有被插入到页面中去，这个元素肯定有了一个内联的样式将来元素肯定会显示到页面中，这时候，浏览器的渲染引擎必然会解析样式，应用给这个元素，和样式相关的操作，一般都可以在 bind 执行

**自定义指令**：用 directive 定义私有指令

```javascript
directives: { // 自定义私有指令
        'fontweight': { // 设置字体粗细的
          bind: function (el, binding) {
            el.style.fontWeight = binding.value
          }
        },
        'fontsize': function (el, binding) { // 注意：这个 function 等同于 把 代码写到了 bind 和 update 中去
          el.style.fontSize = parseInt(binding.value) + 'px'
        }
      }
```

### 生命周期

```javascript
var vm = new Vue({
  el: "#app",
  data: {
    msg: "ok"
  },
  methods: {
    show() {
      console.log("执行了show方法");
    }
  },
  beforeCreate() {
    // 这是我们遇到的第一个生命周期函数，表示实例完全被创建出来之前，会执行它
    // console.log(this.msg)
    // this.show()
    // 注意： 在 beforeCreate 生命周期函数执行的时候，data 和 methods 中的 数据都还没有没初始化
  },
  created() {
    // 这是遇到的第二个生命周期函数
    // console.log(this.msg)
    // this.show()
    //  在 created 中，data 和 methods 都已经被初始化好了！
    // 如果要调用 methods 中的方法，或者操作 data 中的数据，最早，只能在 created 中操作
  },
  beforeMount() {
    // 这是遇到的第3个生命周期函数，表示 模板已经在内存中编辑完成了，但是尚未把 模板渲染到 页面中
    // console.log(document.getElementById('h3').innerText)
    // 在 beforeMount 执行的时候，页面中的元素，还没有被真正替换过来，只是之前写的一些模板字符串
  },
  mounted() {
    // 这是遇到的第4个生命周期函数，表示，内存中的模板，已经真实的挂载到了页面中，用户已经可以看到渲染好的页面了
    // console.log(document.getElementById('h3').innerText)
    // 注意： mounted 是 实例创建期间的最后一个生命周期函数，当执行完 mounted 就表示，实例已经被完全创建好了，此时，如果没有其它操作的话，这个实例，就静静的 躺在我们的内存中，一动不动
  },

  // 接下来的是运行中的两个事件
  beforeUpdate() {
    // 这时候，表示 我们的界面还没有被更新【数据被更新了吗？  数据肯定被更新了】
    /* console.log('界面上元素的内容：' + document.getElementById('h3').innerText)
        console.log('data 中的 msg 数据是：' + this.msg) */
    // 得出结论： 当执行 beforeUpdate 的时候，页面中的显示的数据，还是旧的，此时 data 数据是最新的，页面尚未和 最新的数据保持同步
  },
  updated() {
    console.log("界面上元素的内容：" + document.getElementById("h3").innerText);
    console.log("data 中的 msg 数据是：" + this.msg);
    // updated 事件执行的时候，页面和 data 数据已经保持同步了，都是最新的
  }
});
```

生命周期总的来说分三个部分：数据，挂载，更新，每个部分都有两个阶段：前和后

### vue-resource

getInfo：当发起 get 请求之后， 通过 .then 来设置成功的回调函数，通过 result.body 拿到服务器返回的成功的数据

postInfo：发起 post 请求 , `application/x-wwww-form-urlencoded`，手动发起的 Post 请求，默认没有表单格式，所以，有的服务器处理不了,通过 post 方法的第三个参数， { emulateJSON: true } 设置 提交的内容类型 为 普通表单数据格式

jsonpInfo：发起 JSONP 请求

如果我们通过全局配置了，请求的数据接口 根域名，则 ，在每次单独发起 http 请求的时候，请求的 url 路径，应该以相对路径开头，前面不能带 / ，否则 不会启用根路径做拼接；

## three

### 过渡类名实现动画

1. 使用 transition 元素，把 需要被动画控制的元素，包裹起来，transition 元素，是 Vue 官方提供的
2. 自定义两组样式，来控制 transition 内部的元素实现动画

v-enter 【这是一个时间点】 是进入之前，元素的起始状态，此时还没有开始进入

v-leave-to 【这是一个时间点】 是动画离开之后，离开的终止状态，此时，元素 动画已经结束了

v-enter-active 【入场动画的时间段】

v-leave-active 【离场动画的时间段】

若要修改 V-这个前缀，可以这么做

```css
.my-enter,
.my-leave-to {
  opacity: 0;
  transform: translateY(70px);
}

.my-enter-active,
.my-leave-active {
  transition: all 0.8s ease;
}
```

```html
<transition name="my">
  <h6 v-if="flag2">这是一个H6</h6>
</transition>
```

### 钩子函数实现动画

动画钩子函数的第一个参数：el，表示 要执行动画的那个 DOM 元素，是个原生的 JS DOM 对象，可以认为 ， el 是通过 document.getElementById('') 方式获取到的原生 JS DOM 对象

beforeEnter： 表示动画入场之前，此时，动画尚未开始，可以 在 beforeEnter 中，设置元素开始动画之前的起始样式

enter： 表示动画 开始之后的样式，

done， 起始就是 afterEnter 这个函数，也就是说：done 是 afterEnter 函数的引用

afterEnter 动: 画完成之后，会调用 afterEnter

```html
<transition
  @before-enter="beforeEnter"
  @enter="enter"
  @after-enter="afterEnter"
>
  <div class="ball" v-show="flag"></div>
</transition>
```

```javascript
methods: {
          beforeEnter(el) {
            // 设置小球开始动画之前的，起始位置
            el.style.transform = "translate(0, 0)";
          },
          enter(el, done) {
            // 这句话，没有实际的作用，但是，如果不写，出不来动画效果；
            // 可以认为 el.offsetWidth 会强制动画刷新
            el.offsetWidth;
            el.style.transform = "translate(150px, 450px)";
            el.style.transition = "all 1s ease";
            done();
          },
          afterEnter(el) {
            // console.log('ok')
            this.flag = !this.flag;
          }
        }
```

在实现列表过渡的时候，如果需要过渡的元素，是通过 v-for 循环渲染出来的，不能使用 transition 包裹，需要使用 transitionGroup,如果要为 v-for 循环创建的元素设置动画，必须为每一个 元素 设置 :key 属性,给 ransition-group 添加 appear 属性，实现页面刚展示出来时候，入场时候的效果, 通过 为 transition-group 元素，设置 tag 属性，指定 transition-group 渲染为指定的元素，如果不指定 tag 属性，默认，渲染为 span 标签

```html
<transition-group appear tag="ul">
  <li v-for="(item, i) in list" :key="item.id" @click="del(i)">
    {{item.id}} --- {{item.name}}
  </li>
</transition-group>
```

### 创建组件

#### 方式一

使用 Vue.extend 来创建全局的 Vue 组件

```javascript
var com1 = Vue.extend({
  template: "<h3>这是使用 Vue.extend 创建的组件</h3>" // 通过 template 属性，指定了组件要展示的HTML结构
});
```

使用 Vue.component('组件的名称', 创建出来的组件模板对象)
`Vue.component('myCom1', com1)`
如果使用 Vue.component 定义全局组件的时候，组件名称使用了 驼峰命名，则在引用组件的时候，需要把 大写的驼峰改为小写的字母，同时，两个词之前，使用 - 链接；

如果不使用驼峰,则直接拿名称来使用即可;
`Vue.component('mycom1', com1)`

Vue.component 第一个参数:组件的名称,将来在引用组件的时候,就是一个 标签形式 来引入 它的
第二个参数: Vue.extend 创建的组件 ,其中 template 就是组件将来要展示的 HTML 内容

#### 方式二

```javascript
Vue.component("mycom2", {
  template:
    "<div><h3>这是直接使用 Vue.component 创建出来的组件</h3><span>123</span></div>"
});
```

注意:不论是哪种方式创建出来的组件,组件的 template 属性指向的模板内容,必须有且只能有唯一的一个根元素

#### 方式三

在 被控制的 #app 外面,使用 template 元素,定义组件的 HTML 模板结

```javascript
<template id="tmpl">
  <div>
    <h1>
      这是通过 template
      元素,在外部定义的组件结构,这个方式,有代码的只能提示和高亮
    </h1>
    <h4>好用,不错!</h4>
  </div>
</template>
```

```javascript
Vue.component("mycom3", {
  template: "#tmpl"
});
```

**定义私有组件**

```javascript
components: { // 定义实例内部私有组件的
        login: {
          template: '#tmpl2'
        }
```

### 组件的 data 和 methods

1. 组件可以有自己的 data 数据
2. 组件的 data 和 实例的 data 有点不一样,实例中的 data 可以为一个对象,但是 组件中的 data 必须是一个方法
3. 组件中的 data 除了必须为一个方法之外,这个方法内部,还必须返回一个对象才行;
4. 组件中 的 data 数据,使用方式,和实例中的 data 使用方式完全一样!!!

### 组件切换

#### 方式一

```html
<div id="app">
  <a href="" @click.prevent="flag=true">登录</a>
  <a href="" @click.prevent="flag=false">注册</a>

  <login v-if="flag"></login>
  <register v-else="flag"></register>
</div>
```

#### 方式二

Vue 提供了 component ,来展示对应名称的组件， component 是一个占位符, :is 属性,可以用来指定要展示的组件的名称

```html
<div id="app">
  <a href="" @click.prevent="comName='login'">登录</a>
  <a href="" @click.prevent="comName='register'">注册</a>

  <component :is="comName"></component>
</div>
```

### 组件切换时的动画

通过 mode 属性,设置组件切换时候的 模式

```html
<transition mode="out-in">
  <component :is="comName"></component>
</transition>
```

## four

### 父组件传值给子组件

父组件，可以在引用子组件的时候， 通过 属性绑定（v-bind:） 的形式, 把 需要传递给 子组件的数据，以属性绑定的形式，传递到子组件内部，供子组件使用

子组件中，默认无法访问到 父组件中的 data 上的数据 和 methods 中的方法

子组件中的 data 数据，并不是通过 父组件传递过来的，而是子组件自身私有的，比如： 子组件通过 Ajax ，请求回来的数据，都可以放到 data 身上； data 上的数据，都是可读可写的；

注意： 组件中的 所有 props 中的数据，都是通过 父组件传递给子组件的，props 中的数据，都是只读的，无法重新赋值

`props: ['parentmsg']`把父组件传递过来的 parentmsg 属性，先在 props 数组中，定义一下，这样，才能使用这个数据，只读，写的话会报警告

### 父组件传方法给子组件

父组件向子组件 传递 方法，使用的是 事件绑定机制； v-on, 当我们自定义了 一个 事件属性之后，那么，子组件就能够，通过某些方式，来调用 传递进去的 这个 方法了

当点击子组件的按钮的时候，如何 拿到 父组件传递过来的 func 方法，并调用这个方法？？？

emit 英文原意： 是触发，调用、发射的意思，可以用 emit

### 评论组件

分析：发表评论的业务逻辑

- 评论数据存到哪里去？？？ 存放到了 localStorage 中 localStorage.setItem('cmts', '')
- 先组织出一个最新的评论数据对象
- 想办法，把 第二步中，得到的评论对象，保存到 localStorage 中：
- localStorage 只支持存放字符串数据， 要先调用 JSON.stringify

- 在保存 最新的 评论数据之前，要先从 localStorage 获取到之前的评论数据（string）， 转换为 一个 数组对象， 然后，把最论， push 到这个数组

- 如果获取到的 localStorage 中的 评论字符串，为空不存在， 则 可以 返回一个 '[]' 让 JSON.parse 去转换

- 把 最新的 评论列表数组，再次调用 JSON.stringify 转为 数组字符串，然后调用 localStorage.setItem()

### ref 获取 DOM 元素和组件

ref 是 英文单词 【reference】 值类型 和 引用类型 referenceError

可以通过下面这些方式对应组件和 DOM 中的数据和方法

```
this.$refs.mylogin.msg
this.$refs.mylogin.show()
this.$refs.myh3.innerText
document.getElementById('myh3').innerText
```

### 路由的基本使用

创建一个路由对象， 当 导入 vue-router 包之后，在 window 全局对象中，就有了一个 路由的构造函数，叫做 VueRouter，在 new 路由对象的时候，可以为 构造函数，传递一个配置对象

route : 这个配置对象中的 route 表示 【路由匹配规则】 的意思

每个路由规则，都是一个对象，这个规则对象，身上，有两个必须的属性：属性 1 是 path， 表示监听 哪个路由链接地址；属性 2 是 component， 表示，如果 路由是前面匹配到的 path ，则展示 component 属性对应的那个组件

注意： component 的属性值，必须是一个 组件的模板对象， 不能是 组件的引用名称；

```javascript
var routerObj = new VueRouter({
  routes: [
    // { path: '/', component: login },
    { path: "/", redirect: "/login" }, // 这里的 redirect 和 Node 中的 redirect 完全是两码事
    { path: "/login", component: login },
    { path: "/register", component: register }
  ],
  linkActiveClass: "myactive"
});
```

`<router-view></router-view>`
vue-router 提供的元素，专门用来 当作占位符的，将来，路由规则，匹配到的组件，就会展示到这个 router-view 中去,我们可以把 router-view 认为是一个占位符

router-link 默认渲染为一个 a 标签

```html
<router-link to="/login" tag="span">登录</router-link>
<router-link to="/register">注册</router-link>
```

### 路由规则中定义参数

如果在路由中，使用 查询字符串，给路由传递参数，则 不需要修改 路由规则的 path 属性

#### 方式一

`<router-link to="/login?id=10&name=zs">登录</router-link>`

```javascript
var login = {
  template:
    "<h1>登录 --- {{ $route.query.id }} --- {{ $route.query.name }}</h1>",
  data() {
    return {
      msg: "123"
    };
  }
};
```

#### 方式二

`<router-link to="/login/12/ls">登录</router-link>`

`{ path: '/login/:id/:name', component: login }`

`template: '<h1>登录 --- {{ $route.params.id }} --- {{ $route.params.name }}</h1>'`

### 路由嵌套

使用 children 属性，实现子路由，同时，子路由的 path 前面，不要带 / ，否则永远以根路径开始请求，这样不方便我们用户去理解 URL 地址

```javascript
routes: [
  {
    path: "/account",
    component: account,
    children: [
      { path: "login", component: login },
      { path: "register", component: register }
    ]
  }
];
```

### router-view
```html
<router-view></router-view>
<div class="container">
  <router-view name="left"></router-view>
  <router-view name="main"></router-view>
</div>
```

```javascript
routes: [
  {
    path: "/",
    components: {
      default: header,
      left: leftBox,
      main: mainBox
    }
  }
];
```
### watch属性和computed

watch用来检测变化，一旦data中的数据改变就会执行watch中的操作，可以用来检测路由的变化

```javascript
watch: {
        '$route.path': function (newVal, oldVal) {
          if (newVal === '/login') {
            console.log('欢迎进入登录页面')
          } else if (newVal === '/register') {
            console.log('欢迎进入注册页面')
          }
        }
      }
```

在 computed 中，可以定义一些 属性，这些属性，叫做 【计算属性】， 计算属性的，本质，就是 一个方法，只不过，我们在使用 这些计算属性的时候，是把 它们的 名称，直接当作 属性来使用的；并不会把 计算属性，当作方法去调用；

注意1： 计算属性，在引用的时候，一定不要加 () 去调用，直接把它 当作 普通 属性去使用就好了；
注意2： 只要 计算属性，这个 function 内部，所用到的 任何 data 中的数据发送了变化，就会 立即重新计算 这个 计算属性的值
注意3： 计算属性的求值结果，会被缓存起来，方便下次直接使用； 如果 计算属性方法中，所以来的任何数据，都没有发生过变化，则，不会重新对 计算属性求值；