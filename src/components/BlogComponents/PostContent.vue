<template>
 <div class="card w-100" >
  <div class="card-body">
    <h5 class="card-title">
      <slot name="title"></slot>
    </h5>
    <div class="card-text">
      <slot name="content"></slot>
    </div>
    <div>
      <slot name="toolPanel"></slot>
      <button type="button" class="btn btn-primary my-4" @click="onBack">Back</button>
    </div>

  </div>
</div>
</template>

<script>
// @ is an alias to /src


export default {
  name: 'PostContent',
  components: {

  },
  data: ()=> ({
    searchQuery: ''
  }),
  computed:{
    
  },
  watch:{

  }, 
  methods: {
    onBack() {
      this.$emit('allPosts')
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