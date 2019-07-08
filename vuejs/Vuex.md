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
    state:{
        count:10,
        name:'jack'
    },
    getters:{
        userName(state){
            return state.name + ',Hello'
        }
    },
    mutations:{
        increment(state,num){
            state.count = num
        },
        updateName(state,userName){
            state.name = userName
        }
    },
    actions:{
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
            this.$store.commit('increment',5)
            this.$store.dispatch('incrementAction',5)
        }
    }
})
```

