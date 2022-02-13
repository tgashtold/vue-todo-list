<template>
  <input type="text"  v-model="currentQuery" @input="onQueryChange" placeholder="Rearch">
</template>

<script>

export default {
  name: 'Search',
  props: {value: String},
  data: ()=> ({
    currentQuery: ''
  }),
  computed:{
    
  },
  methods: {
    onQueryChange() {
      this.$emit('queryChanged', this.currentQuery);
    }
  },
  watch: {
    value() {
      this.currentQuery = this.value
    }
  },
  filters: {
  }
}
</script>

<style scoped lang="scss">
</style>