<template>
  <div class="d-flex justify-content-center">
    <div :style="{width:'70%'}">
      <div class="d-flex justify-content-center">
        <h3 class="mb-5">TO DO LIST</h3>
      </div>
      <div class="d-flex justify-content-center mb-4">
        <input type="text" v-model="newToDo" :style="{width:'100%', marginRight:'1rem', padding: '5px 10px'}" placeholder="Enter ToDo">
        <button type="button" class="btn btn-primary" @click="addToDo">Add</button>
      </div>
      <input type="text" :style="{width:'100%',  padding: '5px 10px'}" class="mb-3"  v-model="query" placeholder="Rearch">
      <ul class="list-group" v-if="filteredToDoes.length">
        <li v-for="todo in pageToDoes" class="list-group-item" :key="todo.id">
          <div v-if="!underEditing(todo.id)" class="d-flex  justify-content-between">
            <p v-if="!todo.done">{{todo.toDo}}</p>
            <p v-if="todo.done"><del>{{todo.toDo}}</del></p>
            <div class="btn-group"  role="group" aria-label="Basic example">
              <button type="button"  @click="toggleToDo(todo.id)" class="btn btn-success">{{todo.done ? 'Undone': 'Done'}}</button>
              <button type="button" class="btn btn-primary" @click="editToDo(todo.id)">Edit</button>
              <button type="button" class="btn btn-danger" @click="deleteToDo(todo.id)">Delete</button>
            </div>
          </div>
          <div v-else class="d-flex  justify-content-between">
            <input type="text" v-model="todo.toDo" :style="{width:'100%', marginRight:'1rem', padding: '5px 10px'}" placeholder="ToDo">
            <div class="btn-group" role="group" aria-label="Basic example">
              <button type="button" class="btn btn-primary" @click="saveToDo(todo.id)">Save</button>
              <button type="button" class="btn btn-danger" @click="cancelToDo(todo.id)">Cancel</button>
            </div>
          </div>
        </li>
      </ul>
      <div v-else>
        <p>No Items In ToDo List</p>
      </div>
      <nav aria-label="Page navigation example" v-if="filteredToDoes.length" class="mt-3">
        <ul class="pagination justify-content-center">
          <li :class="{'page-item': true, disabled: currentPage === 1}" @click="prevPage"><a class="page-link" href="#">Prev</a></li>
          <li :class="{'page-item': true, active: page === currentPage}" v-for="page in pagesCount" :key="page" @click="switchPage(page)"><a class="page-link"  >{{page}}</a></li>
          <li :class="{'page-item': true, disabled: currentPage === pagesCount}" @click="nextPage"><a class="page-link" href="#">Next</a></li>
        </ul>
      </nav>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
const API_URL = 'https://my-json-server.typicode.com/tgashtold/vue-todo-list/todos';
const itemsPerPage = 10;

export default {
  name: 'Home',
  data: ()=> ({
    toDoes: [],
    newToDo: '',
    editedToDoes: [],
    query: '',
    currentPage: 1
  }),
  mounted: function() {
     axios.get(API_URL).then((res) => {
      this.toDoes = res.data;
    }).catch((err) => {
      console.log(err)
    })
  },
  methods: {
    prevPage() {
      if(this.currentPage !== 1){
        this.currentPage = --this.currentPage;
      }
    },
    nextPage() {
      if (this.currentPage !== this.pagesCount) {
        this.currentPage = ++this.currentPage;
      }
    },
    switchPage(page) {
      this.currentPage = page;
    },
    addToDo(){
      const toDo = this.newToDo.trim();

      if (toDo.length) {
        this.toDoes.unshift({
          toDo,
          done:false,
          id: Math.random() * 10000
        });
        this.newToDo = '';
      }
    },
    deleteToDo(id) {
      this.toDoes = this.toDoes.filter(todo => todo.id !== id);
      if(this.currentPage>this.pagesCount) {
        this.currentPage = --this.currentPage;
      }
    },
    toggleToDo(id){
      this.toDoes = this.toDoes.map(todo => {
        if(todo.id === id) {
          todo.done = !todo.done;
        }
        return todo;
        });
    },
    editToDo(id){
      const editedToDo = this.toDoes.find(todo => todo.id === id);
      this.editedToDoes.push(JSON.parse(JSON.stringify(editedToDo)));
    },
    underEditing(id) {
      return this.editedToDoes.some(todo => todo.id === id);
    },
    saveToDo(id) {
      this.editedToDoes = this.editedToDoes.filter(todo => todo.id !== id);
      this.currentPage = 1;
    },
    cancelToDo(id) {
      this.editedToDoes = this.editedToDoes.filter(edittodo => {
        const underEditing = edittodo.id === id;
        if(underEditing) {
          this.toDoes = this.toDoes.map(todo => {
            return todo.id === id ? edittodo : todo;
          })
        }
        
      })
    }
  },
  computed: {
    filteredToDoes() {
      const query = this.query.toLowerCase().trim();

      return query.length > 3 
        ?  this.toDoes.filter(todo => todo.toDo.toLowerCase().includes(query))
        :  this.toDoes;
    },
    pagesCount() {
      const items = this.filteredToDoes.length;
      return items ? Math.ceil(items/itemsPerPage) : null;
    },
    pageToDoes() {
      return this.filteredToDoes.slice((this.currentPage-1)*itemsPerPage, this.currentPage*itemsPerPage);
    }
  },
}
</script>

<style scoped lang="scss">
</style>