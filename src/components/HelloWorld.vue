<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h1>列表共有{{ list.length }}项事件，未完成{{item_undone}}项，已完成{{item_done}}项</h1>
    <h1>    
      <input v-model="message"/> 
      <button @click="insert">提交</button>
    </h1>


    <h1 v-for="(item, index) of list" v-bind:key="index">{{ index+1 }}.{{ item.name }}:
      
      <li v-if="item.done!=0">已完成 
        <button @click="update0(item.id)">设为未完成</button>
      </li>
        
      <li v-else>未完成   
        <button @click="update1(item.id)">设为已完成</button>
      </li>
    </h1>

  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    
    return{
      
      message:'请输入代办内容',
      list:[],
      item_undone:0,
      item_done:0

    }
  },
  methods:{
    getlist(){
      axios({
        method: 'get',
      //url:'https://mock.apifox.cn/m1/2014704-0-default/todo/list',
      url:'http://127.0.0.1:48080/getTodoList',
      }).then((res) => {
      //this.list = res.data.data.rows;
      this.list = res.data;
        console.log(this.list)
      });
    },

    update0(input){
      axios({
        method: 'post',
      url:'http://127.0.0.1:48080/update',
      data:{
        id: input,
        done:0
      }
      }).then(function(response) {
        console.log(response);
      }).then(function(error){
        console.log(error)
      });
      this.item_undone = this.item_undone + 1;
      this.item_done = this.item_done - 1;
    },
    update1(input){
      axios({
        method: 'post',
      url:'http://127.0.0.1:48080/update',
      data:{
        id:input,
        done:1
      }
      }).then(function(response) {
        console.log(response);
      }).then(function(error){
        console.log(error)
      });
      this.item_undone = this.item_undone - 1;
      this.item_done = this.item_done + 1;
    },
    insert(){
      axios({
        method: 'post',
      url:'http://127.0.0.1:48080/insert',
      data:{
        id:this.list.length,
        name:this.message,
        done:0
      }
      }).then(function(response) {
        console.log(response);
      }).then(function(error){
        console.log(error)
      });
      this.message="";
    },
  
  },
  

  created(){
    this.getlist();

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
