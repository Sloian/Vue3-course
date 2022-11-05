<template>
  <div class="app">
    <h1>Сторінка з постами</h1>
    <div class="app_btns">
      <my-button @click="showDialog" class="btn-create-post">Створити пост</my-button>
      <my-select
        v-model="selectedSort"
        :options="sortOptions"
      />
    </div>


    <my-dialog v-model:show="dialogVisible" >
      <PostForm  @create="createPost"/>
    </my-dialog>

    <PostList
      :posts="sortedPosts"
      @remove="removePost"
      v-if="!isPostsLoading"
    />
    <div v-else>Йде загрузка</div>

  </div>

</template>


<script>
import PostForm from './components/PostForm.vue';
import PostList from './components/PostList.vue';
import MyDialog from './components/UI/MyDialog.vue';
import MySelect from './components/UI/MySelect.vue';
import axios from 'axios';


export default {

  components:{
    PostForm,
    PostList,
    MyDialog,
    MySelect,
},

  data() {
    return {
      posts:[],
      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: '',
      sortOptions:[
        {value: 'title', name: 'По назві'},
        {value: 'body', name: 'По опису'},
      ]
    }
  },

  methods: {
    createPost(post){
      this.posts.push(post)
      this.dialogVisible = false;
    },
    removePost(postDel){
      console.log(postDel)
      this.posts = this.posts.filter(post => post.id !== postDel.id)
    },
    showDialog(){
      this.dialogVisible = true;
    },
    async fetchPosts(){
      try {
        this.isPostsLoading = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10')
        this.posts = response.data;
      } catch (error) {
        alert('Виникла помилка')
      } finally{
        this.isPostsLoading = false
      }
    }
  },
  mounted(){
    this.fetchPosts();
  },
  computed:{
    sortedPosts(){
      return [...this.posts].sort((post1, post2) =>{
        return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
      })
    }
  }
  ,
  watch: {
    // selectedSort(newValue){
    //   console.log(newValue)
    //   this.posts.sort((post1, post2) =>{
    //     return post1[newValue]?.localeCompare(post2[newValue])
    //   })
    // },
    // dialogVisible(newValue){
    //   console.log(newValue)
    // }
  }
}

</script>


<style>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app{
  padding: 20px;
}

.app_btns{
  display: flex;
  justify-content: space-between;
  margin: 15px 0;
}

</style>
