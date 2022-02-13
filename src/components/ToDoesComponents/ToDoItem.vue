<template>
        <div>
          <div v-if="!editing" class="d-flex  justify-content-between">
            <slot name="toDoText"></slot>
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
        </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'ToDoItem',
  props: {todo: {
    id: Number,
    toDo: String,
    done: Boolean

  }},
  data: ()=> ({
    editing: false,
  }),
  computed:{
    
  },
  methods: {
    editToDo () {
      this.$emit('edit', this.todo.id)
      this.editing = true;
    },
    toggleToDo () {
      this.$emit('toggle', this.todo.id)
    },
    deleteToDo () {
       this.$emit('delete', this.todo.id)
    },
    saveToDo () {
       this.$emit('save', this.todo.id);
       this.editing = false;
    },
    cancelToDo () {
       this.$emit('cancel', this.todo.id);
       this.editing = false;

    }
  },
  filters: {
  }
}
</script>

<style scoped lang="scss">
header {
  background-color: lightsteelblue;
  height: 70px;
}
</style>