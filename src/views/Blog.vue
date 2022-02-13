<template>
 <div>
  <div v-if="!currentPost">
    <button type="button" class="btn btn-secondary mb-3" @click="toggleTools">{{ showTools ? "Hide Tools" : "Show Tools"}}</button>
    <ToolPanel v-if="showTools" @search="onSearch" />
    <div class="d-flex flex-wrap p-5 justify-content-around">
      <BlogPost  v-for="post in filteredPosts" :key="post.id" @readMore="onPostClick(post)" :post="post" @like="changeLikes(post)" class="m-2"/>
    </div>
   
  </div>
   <div v-if="currentPost" class="d-flex flex-wrap p-5 justify-content-around">
      <PostContent @allPosts="showAllPosts">
        <template v-slot:title>{{currentPost.title}}</template>
        <template v-slot:content>
          <p class="text-muted">{{currentPost.desctiption}}</p>
          <p>Sed ut perspiciatis, unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam eaque ipsa, quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt, explicabo. Nemo enim ipsam voluptatem, quia voluptas sit, aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos, qui ratione voluptatem sequi nesciunt, neque porro quisquam est, qui dolorem ipsum, quia dolor sit, amet, consectetur, adipisci velit, sed quia non numquam eius modi tempora incidunt, ut labore et dolore magnam aliquam quaerat voluptatem. Ut enim ad minima veniam, quis nostrum exercitationem ullam corporis suscipit laboriosam, nisi ut aliquid ex ea commodi consequatur? Quis autem vel eum iure reprehenderit, qui in ea voluptate velit esse, quam nihil molestiae consequatur, vel illum, qui dolorem eum fugiat, quo voluptas nulla pariatur? At vero eos et accusamus et iusto odio dignissimos ducimus, qui blanditiis praesentium voluptatum deleniti atque corrupti, quos dolores et quas molestias excepturi sint, obcaecati cupiditate non provident, similique sunt in culpa, qui officia deserunt mollitia animi, id est laborum et dolorum fuga. Et harum quidem rerum facilis est et expedita distinctio. Nam libero tempore, cum soluta nobis est eligendi optio, cumque nihil impedit, quo minus id, quod maxime placeat, facere possimus, omnis voluptas assumenda est, omnis dolor repellendus. Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus saepe eveniet, ut et voluptates repudiandae sint et molestiae non recusandae. Itaque earum rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus maiores alias consequatur aut perferendis doloribus asperiores repellat.</p>
        </template>
        <template v-slot:toolPanel>
          <p><small class="text-muted">{{currentPost.date.toLocaleDateString()}}</small></p>
        </template>
      </PostContent>
    </div>
  </div>
</template>

<script>

import ToolPanel from '@/components/BlogComponents/ToolPanel.vue'
import BlogPost from '@/components/BlogComponents/BlogPost.vue'
import PostContent from '@/components/BlogComponents/PostContent.vue'

export default {
  name: 'Blog',

  components: {
    ToolPanel,
    BlogPost,
    PostContent
  },
  data: ()=> ({
    query: 'dfs',
    showTools: true,
    currentPost: null,
    posts: [{
      id:1,
      title: 'Test Post1',
      desctiption: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      date: new Date(),
      watched: 200,
      likes: 20
    },
    {
      id:2,
      title: 'Test Post2',
      desctiption: 'Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.n',
      date: new Date(),
      watched: 73,
      likes: 23
    },
    {
      id:3,
      title: 'Test Post3',
      desctiption: 'Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      date: new Date(),
      watched: 54,
      likes: 12
    },
    {
      id:4,
      title: 'Test Post4',
      desctiption: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua',
      date: new Date(),
      watched: 434,
      likes: 234
    },
    {
      id:5,
      title: 'Test Post5',
      desctiption: 'Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      date: new Date(),
      watched: 111,
      likes: 23
    },
    {
      id:6,
      title: 'Test Post6',
      desctiption: 'Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      date: new Date(),
      watched: 345,
      likes: 234
    },
    {
      id:7,
      title: 'Test Post7',
      desctiption: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.',
      date: new Date(),
      watched: 676,
      likes: 345
    },
    {
      id:8,
      title: 'Test Post8',
      desctiption: 'Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      date: new Date(),
      watched: 4456,
      likes: 2345
    }].map(post => {
      post.userLiked = false;
      return post
    })
  }),
  computed:{
    filteredPosts(){
      const query = this.query.toLowerCase().trim();

      return query.length > 3
        ? this.posts.filter(post => {
            return post.title.toLowerCase().includes(query);
          })
        : this.posts;
    }
  },
  methods: {
    onPostClick(post) {
      this.currentPost=post;
    },
    toggleTools() {
      this.showTools = !this.showTools;
    },
    changeLikes(post) {
      post.userLiked ? --post.likes : ++post.likes;
      post.userLiked = !post.userLiked;
    },
    onSearch(value) {
      this.query = value;
    },
    showAllPosts(){
      this.currentPost = null;
    }
    
  },
  filters: {
  }
}
</script>

<style scoped lang="scss">

</style>