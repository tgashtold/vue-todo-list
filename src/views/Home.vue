<template>
  <div class="d-flex justify-content-center pb-5">
    <div :style="{width:'70%'}">
      <div class="d-flex justify-content-center">
        <h3 class="mb-5">TO DO LIST</h3>
      </div>

      <NewToDo class="mb-4" @newToDo="addToDo"/>
        <!-- <input type="text" v-model="newToDo" :style="{width:'100%', marginRight:'1rem', padding: '5px 10px'}" placeholder="Enter ToDo">
        <button type="button" class="btn btn-primary" @click="addToDo">Add</button>
      </div> -->
      <Search 
        :style="{width:'100%',  padding: '5px 10px'}" 
        class="mb-3"
        @queryChanged="onSearch"
        v-model="query"
      />
      <button type="button" class="btn btn-info my-3 w-100" @click="switchMode">{{lazyLoading ? "Pagination" : "Infinite Scroll"}}</button>
    <div>
      <ul class="list-group"  v-if="filteredToDoes.length">
        <li 
          is="ToDoItem"  
          v-for="todo in pageToDoes" 
          class="list-group-item" 
          :key="todo.id" 
          :todo="todo"
          @toggle="toggleToDo"
          @save="saveToDo"
          @delete="deleteToDo"
          @cancel="cancelToDo"
          @edit="editToDo"
        >
        <template v-slot:toDoText>
          <p v-if="!todo.done" :class="{'text-danger': todo.toDo.toLowerCase().includes('urgent') }">{{todo.toDo}}</p>
          <p v-if="todo.done"><del>{{todo.toDo}}</del></p>
        </template>
        </li>
     
      </ul>
      <div v-else>
        <p>No Items In ToDo List</p>
      </div>
         <div ref="preloader"  class="d-flex justify-content-center">
           <div v-if="filteredToDoes.length && lazyLoading && loading" class="spinner-border text-dark my-3" role="status">
            <span class="sr-only"></span>
          </div>
         </div>
      </div>
      <Pagination 
        v-if="filteredToDoes.length && !lazyLoading" 
        class="mt-3"
        :currentPage="currentPage"
        :pagesCount="pagesCount"
        @pageChange="switchPage"
      />

    </div>
  </div>
</template>

<script>
import axios from 'axios';
import ToDoItem from '@/components/ToDoesComponents/ToDoItem.vue'
import Pagination from '../components/ToDoesComponents/Pagination.vue';
import Search from '../components/ToDoesComponents/Search.vue';
import NewToDo from '../components/ToDoesComponents/NewToDo.vue';

const API_URL = 'https://my-json-server.typicode.com/tgashtold/vue-todo-list/todos';
const itemsPerPage = 10;

export default {
  name: 'Home',
  components:{ ToDoItem, 
  Pagination, 
  Search, NewToDo},
  data: ()=> ({
    lazyLoading: true,
    loading: false,
    toDoes: [],
    newToDo: '',
    editedToDoes: [],
    query: '',
    currentPage: 1,
    observer: null
  }),
  mounted: function() {
     axios.get(API_URL).then((res) => {
      this.toDoes = res.data;
      this.initPreloader();
    }).catch((err) => {
      console.log(err)
    })
  },
  methods: {
    switchMode() {
      this.lazyLoading = !this.lazyLoading;
      this.currentPage = 1;
      if (this.lazyLoading) {
        this.observer.observe(this.$refs.preloader);
      }
      else {
        this.observer.unobserve(this.$refs.preloader);
      }

    },
    initPreloader() {
      const todoList = this;

      this.observer = new IntersectionObserver(function(entries) {
        if (entries[0].isIntersecting && todoList.filteredToDoes.length > todoList.pageToDoes.length) {
          todoList.loading = true;

          setTimeout(() => {
            todoList.loading = false;
            ++todoList.currentPage;
          }, 2000)
          console.log(666, entries)
        }
      }, { threshold: 1});

      if (this.lazyLoading) {
        this.observer.observe(this.$refs.preloader);
      }

    },
    onSearch(value) {
      this.query = value;
    },
    switchPage(page) {
      this.currentPage = page;
    },
    addToDo(todo){
      this.toDoes.unshift(todo);
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
      return items ? Math.ceil(items/itemsPerPage) : 0;
    },
    pageToDoes() {
      if(!this.lazyLoading) {
        console.log(777)
      return this.filteredToDoes.slice((this.currentPage-1)*itemsPerPage, this.currentPage*itemsPerPage);
      }
      console.log(888, this.currentPage)
      return this.filteredToDoes.slice(0, this.currentPage*itemsPerPage)
    }
  },
}
</script>

<style scoped lang="scss">
</style>