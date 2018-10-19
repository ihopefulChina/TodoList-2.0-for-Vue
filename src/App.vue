<template>
  <div id="app">
    <h1>TodoList for Vue</h1>
    <input type="text" v-model='todo'  @keydown="doAdd($event)" placeholder="添加TodoList" >
    <h2>进行中</h2>
    <ul>
      <li v-for="(item,key) in list" v-if="!item.checked">
        <input type="checkbox" v-model="item.checked" @change="saveList()">{{item.title}}<button @click="doDel(key)">删除</button>
      </li>
    </ul>
    <h2>已完成</h2>
    <ul>
      <li v-for="(item,key) in list" v-if="item.checked" class="blue">
        <input type="checkbox" v-model="item.checked" @change="saveList()">{{item.title}}<button @click="doDel(key)">删除</button>
      </li>
    </ul>
  </div>
</template>

<script>
import storage from './model/storage.js';

export default {
  name: 'app',
  data () {
    return {
     todo:'',
     list:[]
    }
  },
  methods:{
      doAdd(e){
        if(e.keyCode==13){
        this.list.push({
          title: this.todo,
          checked: false,
        })
        this.todo=''
        }
        storage.set('list',this.list);
      },
      doDel(key){
        this.list.splice(key,1)
        storage.set('list',this.list);
      },
      saveList(){
       storage.set('list',this.list);
      }
    },
  mounted(){
    var list=storage.get('list');;
        if(this){
          this.list=list;
        }
      }
  }
</script>

<style lang="scss">
  #app{
    text-align: center;
    width: 60%;
    margin: 0 auto;
  }
  input{
    padding: 15px;
    width: 80%;
    margin: 0 auto;
  }
  ul li input[type="checkbox"]{
    width: auto;
    padding: 5px;
    margin-right: 15px;
  }
  li{
    list-style: none;
    color: red;
    font-weight: 700;
    text-align: left;
    padding: 15px;
  }
  button{
    float: right;
    border: none;
    background: red;
    padding: 5px;
    color: #fff
  }
  .blue{
    color:blue;
  }
  .box{
    height: 100px;
    width: 100px;
    background: red
  }
</style>
