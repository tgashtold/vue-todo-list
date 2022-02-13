<template>
    <nav aria-label="Page navigation example">
      <ul class="pagination justify-content-center">
        <li :class="{'page-item': true, disabled: currentPage === 1}" @click="prevPage"><a class="page-link" href="#">Prev</a></li>
        <li :class="{'page-item': true, active: page === currentPage}" v-for="page in pagesCount" :key="page" @click="switchPage(page)"><a class="page-link"  >{{page}}</a></li>
        <li :class="{'page-item': true, disabled: currentPage === pagesCount}" @click="nextPage"><a class="page-link" href="#">Next</a></li>
      </ul>
    </nav>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Pagination',
  props: {pagesCount: Number, currentPage: Number},
  data: ()=> ({
  }),
  computed:{
    
  },
  methods: {
    switchPage(page) {
      this.$emit('pageChange', page);
    },
    prevPage() {
      if (this.currentPage !== 1) {
        this.$emit('pageChange', this.currentPage - 1);
      }
    },
    nextPage() {
      if (this.currentPage !== this.pagesCount) {
        this.$emit('pageChange', this.currentPage + 1);
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