<template>

  <MySelect :options="options" v-model="selected" />

  <MyInput v-model="searchQuery" />

  <MyPagination :totalPage="totalPage" :curPage="currentPage" @updatePage="updatePage"/>

  <div v-if="!searchPost.length">not match</div>

  <transition-group name="posts">
    <div class="block" v-for="post in searchPost" :key="post.id">
      <div class="block__item"><strong>id: </strong> {{post.id}}</div>
      <div class="block__item"><strong>Title: </strong>{{post.title}}</div>
      <div class="block__item"><strong>Body: </strong> {{post.body}}</div>
      <button @click="deletePost(post)" style="margin:10px 10px">Delete</button>
    </div>
  </transition-group>
</template>

<script>
import axios from 'axios';
import MyPagination from './components/MyPagination.vue'
import MyInput from './components/MyInput.vue';
import MySelect from './components/MySelect.vue';
export default {
  name: 'App',
  components: {
    MyInput,
    MySelect,
    MyPagination,
  },
  data() {
    return {
      searchQuery: '',
      posts: [],
      selected: '',
      currentPage: 1,
      limit: 10,
      totalPage: 0,
      options: [
        {id: 'title', title: 'Поиска по названию'},
        {id: 'id', title: 'Поиска по номеру'},
      ]
    }
  },
     methods: {
       updatePage(page){
         console.log('e' , page)
         this.currentPage = page;
         this.fetchPosts();
       },
       deletePost(e){
        this.posts = this.posts.filter(post => post.id !== e.id);
       },
       async fetchPosts(){
        let response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.currentPage,
            _limit: this.limit
          }
        });
        this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
        console.log('totalPage',this.totalPage);
        this.posts = response.data;
       },

     },
     computed: {
       searchPost() {
         return this.selectedSort.filter(el => el.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
       },
        selectedSort(){
         return [...this.posts].sort((a, b) => a[this.selected] > b[this.selected] ? 1 : -1);

        },
     },
   
    mounted(){
      this.fetchPosts();
    },
}
</script>

<style>
  #app{
    display: flex;
    flex-direction: column;
    max-width: 400px;
  }
  .block{
    border: 1px solid teal;
    margin: 20px;
  }
  .block__item{
    padding: 10px 10px 10px;
    border: 1px solid grey;
  }
  
  select , input{
    max-width: 250px;
    margin-top: 20px;
  }
.posts-item {
  display: inline-block;
  margin-right: 10px;
}
.posts-enter-active,
.posts-leave-active {
  transition: all 1s ease;
}
.posts-enter-from,
.posts-leave-to {
  opacity: 0;
  transform: translateY(30px);
}
.posts-move {
  transition: transform 0.8s ease;
}
</style>
