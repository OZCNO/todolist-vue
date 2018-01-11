<template>
  <div id="app">
    <h1 v-text="title" v-bind:title="title"></h1>
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <ul>
      <li v-for="item in items" v-bind:class="{finished:item.isFinished}" v-on:click="toggleFinish(item)">
        {{item.label}}
      </li>
    </ul>
  </div>
</template>    

<script>
import Store from "./store.js"
export default {
  name: 'app',
  data:function(){
      return{
        title:"this is a todo list",
        newItem:"",
        // items:Store.fetch(),
        items:Store.fetch()
      }
  },
  //methods写差个s，总是提醒我他们不是函数，找了一个多小时....注意了！！！
  methods:{

    addNew:function(){
      this.items.push({
        label:this.newItem,
        isFinished:false
      })
      this.newItem=""
    },
    
    toggleFinish:function(item){
      item.isFinished=!item.isFinished
    }
  },
  watch:{
    //监听items变化
    items:{
      handler:function(items){
        Store.save(items)
      },
      //设置deep为true，当属性变化也能监听到
      deep:true
    }
  }

}
</script>

<style>
#app {
  text-align: center;
}
.finished{
  color:lightgrey;
}
</style>
