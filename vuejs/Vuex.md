```html
<div id="app">
	<counter></counter>			
</div>
```

```javascript
const counter ={
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
```

