<template>
  <header class="w-100 p-3">
    <CustomSearch v-model="searchQuery" class="w-100"/>
  </header>
</template>

<script>
// @ is an alias to /src
import CustomSearch from './CustomSearch.vue'

export default {
  name: 'ToolPanel',
  components: {
    CustomSearch
  },
  data: ()=> ({
    searchQuery: ''
  }),
  computed:{
    
  },
  watch:{
    searchQuery() {
      this.$emit('search', this.searchQuery);
    }
  }, 
  methods: {

    
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