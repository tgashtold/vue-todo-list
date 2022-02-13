<template>
      <div class="d-flex justify-content-center">
        <input type="text" v-model="newToDo" :style="{width:'100%', marginRight:'1rem', padding: '5px 10px'}" placeholder="Enter ToDo">
        <button type="button" class="btn btn-primary" @click="addToDo">Add</button>
      </div>
</template>

<script>

export default {
  name: 'NewToDo',
  props: {},
  data: ()=> ({
    newToDo: ''
  }),
  computed:{

  },
  methods: {
    addToDo() {
      const toDo = this.newToDo.trim();

      if (toDo.length) {
        this.$emit('newToDo', {
          toDo,
          done:false,
          id: Math.random() * 10000
        });
        this.newToDo = '';
      }
    }
  },
  watch: {
  },
  filters: {
  }
}
</script>

<style scoped lang="scss">
</style>