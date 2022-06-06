```html
<div id="app">
  <counter></counter>
</div>
```

```javascript
{% raw %}
const counter = {
  template: `
    <div>
    <div>点击数量：{{count}}</div>
    <div>用户名：{{name}}</div>
    </div>
`,
  computed:{
    count(){
      return this.$store.state.count
    },
    name(){
      return this.$store.state.name
    },
    userName(){
      return this.$store.getters.userName
    }
  }
}
const store = new Vuex.Store({
  state:{ // 定义状态，相当于 data
    count:10,
    name:'jack'
  },
  getters:{ // 与 state 一样是 状态，相当与 computed
    userName(state){
      // 通过 store.getters.userName 调用
      return state.name + ',Hello'
    }，
    getTodoById: (state) => (id) => {
      // 也可以返回一个函数，通过 store.getters.getTodoById(2) 调用
      return state.todos.find(todo => todo.id === id)
    }
  },
  mutations:{ // 更改 state 内的值，只能同步操作 通过 this.$store.commit('increment',5)
    increment(state,num){
      state.count = num
    },
    updateName(state,userName){
      state.name = userName
    }
  },
  actions:{ // 触发 mutations 的函数，可异步操作 通过 this.$store.dispatch('incrementAction',5) 触发
    incrementAction(context,num){
      context.commit('increment',num)
    }
  }
})
new Vue({
  el:"#app",
  store,
  components:{
    counter
  },
  methods:{
    add(){
      this.$store.commit('increment',5) // 触发 mutations 的函数
      this.$store.dispatch('incrementAction',5) // 触发 actions 的函数
    }
  }
})
{% endraw %}
```


## vuex 支持事件发布-订阅

```js
// index.js
import events from './events'

export default function (store) {
  // store.prototype = store.prototype || {}
  events.mixTo(store)
  store.registerModule('myEvents', {
    mutations: {
      setEvent () {
      }
    }
  })
  store.$emit = function (evt, ...arg) {
    if (!this.events[evt]) return
    this.trigger(evt, ...arg)
    this.commit('setEvent', evt)
  }
}
```

```js
// events.js
/* eslint-disable */

function Events () {
}

Events.prototype.events = {}
Events.prototype.on = function (evt, callback) {
  if (!callback || !evt) return this
  this.events[evt] = this.events[evt] || []
  this.events[evt].push(callback)
  return this
}

Events.prototype.once = function (evt, callback) {
  let that = this
  let cb = function () {
    that.off(evt, cb)
    callback(arguments)
  }
  return this.on(evt, cb)
}

Events.prototype.off = function (evt, callback) {
  if (!evt) {
    return this
  }
  let events = this.events[evt]
  if (!callback) {
    delete this[evt]
  } else {
    for (let i = events.length; i--;) {
      if (events[i] === callback) {
        events.splice(i, 1)
        return this
      }
    }
  }
}

Events.prototype.trigger = function (evt, ...arg) {
  let events = this.events[evt]
  if (!evt || !events) return this
  let len = events.length
  for (let i = 0; i < len; i++) {
    events[i](...arg)
  }
}

Events.prototype.emit = Events.prototype.trigger

Events.mixTo = function (receiver) {
  var proto = Events.prototype

  if (isFunction(receiver)) {
    for (var key in proto) {
      if (proto.hasOwnProperty(key)) {
        receiver.prototype[key] = proto[key]
      }
    }
  } else {
    for (var key in proto) {
      if (proto.hasOwnProperty(key)) {
        receiver[key] = proto[key]
      }
    }
  }
}


function isFunction (func) {
  return Object.prototype.toString.call(func) === '[object Function]'
}

export default Events
```

```js
// store.js
import Vue from 'vue'
import Vuex from 'vuex'

export default new Vuex.Store({
})

```

```js
// main.js
import store from 'store.js'
import vueEvent from 'store/events/index.js'

vueEvent(store)
```