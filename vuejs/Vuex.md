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
    getters:{
        userName(state){
            return state.name + ',Hello'
        }
    },
    mutations:{ // 更改 state 内的值，只能同步操作
        increment(state,num){
            state.count = num
        },
        updateName(state,userName){
            state.name = userName
        }
    },
    actions:{ // 触发 mutations 的函数，可异步操作
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

