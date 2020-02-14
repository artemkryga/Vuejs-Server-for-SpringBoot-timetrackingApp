<template>
<div id="app">
  <h1></h1>
  
  <table class="table" >
    <tr>
      <th>Category</th>
      <th>Activity</th>
      <th>Time</th>
      <th>Action</th>
      
    </tr>
  <tr
    v-for="currency in todos " v-bind:key="currency.nameAct"
  >
  <td>
    <h5>{{currency.nameCateg}}</h5>
    </td>
  <td>
    <h5>{{currency.nameAct}}</h5>
    </td>
<td>
    <input type="text" :id="currency.nameAct" />
</td>
<td>
    <input class="btn btn-primary" type="button" value="Click me" @click="addTodo(currency)">
</td>
  </tr>
  </table>
</div>
  
</template>


<script>
import axios from 'axios';
import UserService from '../services/user.service';
const API_URL = 'http://192.168.0.105:8080/api/test/';
export default {
  name: 'user',
  data() {
    return {
      todos: [],
      todoName: '' ,
    
    };
  },
   
  computed: {
    currentUser() {
      return this.$store.state.auth.user;
    } 

  },

async created() {
    
    try{
      const res = await axios.post(API_URL +'getUserAct', { nameUser: this.currentUser.username });

      this.todos = res.data;
        }catch(e) {
        UserService.getPublicContent.then(error =>  {
        this.todoName = error.response.data.message;
      })
      } 
        } ,
        methods: {
  addTodo(currency) {
           try{
                         alert(document.getElementById(currency.nameAct).value);
          axios.post(API_URL +'addStatistics', {  time: document.getElementById(currency.nameAct).value ,  nameActivity: currency.nameAct , nameUser: this.currentUser.username });

        }catch(e) {
        UserService.getPublicContent.then(error =>  {
        this.todoName = error.response.data.message;
      })
      } 
        } 
        }
};

</script>