<template>
  <div class="d-flex justify-content-center">
    <div :style="{width:'70%'}">
      <div class="d-flex justify-content-center">
        <h3 class="mb-5">TO DO LIST</h3>
      </div>
      <div class="d-flex justify-content-center mb-4">
        <input type="text" :style="{width:'100%', marginRight:'1rem', padding: '5px 10px'}" placeholder="Enter ToDo">
        <button type="button" class="btn btn-primary">Add</button>
      </div>
      <input type="text" :style="{width:'100%',  padding: '5px 10px'}" class="mb-3" placeholder="Rearch">
      <ul class="list-group">
        <li class="list-group-item">
          <div class="d-flex  justify-content-between">
            <p>clean room</p>
            <div class="btn-group" role="group" aria-label="Basic example">
              <button type="button" class="btn btn-success">Done</button>
              <button type="button" class="btn btn-primary">Edit</button>
              <button type="button" class="btn btn-danger">Delete</button>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
const API_URL = 'https://my-json-server.typicode.com/tgashtold/json-server';
const requestUrls = {
  db: API_URL +'/db',
  todos: API_URL +'/posts',
}
// const defaultToDos = [
//   {toDo: 'Clean Room', done: false},
//   {toDo: 'Feed Cat', done: true},
//   {toDo: 'Find hotel in Giorgia', done: false}
// ];

export default {
  name: 'Home',
  // components: {
  //   HelloWorld
  // }
  data: ()=> ({
    todos: []
  }),
  mounted: function() {
    axios.post(requestUrls.todos, {toDo: 'Clean Room', done: false}).then((res) => {
      console.log(111,res)
    }).catch((err) => {
      console.log(err)
    })

 axios.get(requestUrls.todos).then((res) => {
      console.log(111,res)
    }).catch((err) => {
      console.log(err)
    })
  },
  methods: {
    // prevEmployee(){
    //   if(this.display > 0){
    //     this.display --
    //   }
    // },
    // nextEmployee(){
    //   if(this.display < (this.employees.length-1)){
    //     this.display ++
    //   }
    // },
    // showDetails(index) {
    //   this.display = index;
    // },
    // toggleList() {
    //   this.showList = !this.showList;
    //   this.display = undefined;
    // }
  },
  filters: {
    // capitalize(name) {
    //   return name.split(/\s+/).map(part => part.charAt(0).toUpperCase() + part.toLowerCase().slice(1)).join(' ');
    // } 
  }


}
</script>

<style scoped lang="scss">
// .card {
//   border: 2px green solid;
//   margin-top: 10px;
//   padding: 1rem;
// }
</style>