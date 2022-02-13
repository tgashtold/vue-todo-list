<template>
 <div class="card"  style="width: 18rem;">
  <div class="card-body">
    <h5 class="card-title mb-3">{{post.title}}</h5>
    <h6 class="card-subtitle mb-2 text-muted">{{post.date.toLocaleDateString()}}</h6>
    <p class="card-text">{{post.desctiption}}</p>
    <p class="card-subtitle mb-2 text-muted">Watched {{post.watched}} times</p>
    <button :class="{btn:true, 'btn-sm':true, 'btn-secondary': !post.userLiked, 'btn-success': post.userLiked}" @click="onLike">Likes: 
      <span class="badge bg-warning text-dark">{{post.likes}}</span>
    </button>
    <div @click="onReadMore" class="m-1 text-primary" >Read More ...</div>
  </div>

</div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'BlogPost',
  props: ['post'],
  // components: {
  //   HelloWorld
  // }
  data: ()=> ({
  }),
  computed:{
    
  },
  methods: {
    onLike() {
      this.$emit('like');
    },
    onReadMore(){
      this.$emit('readMore')
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