<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <my-input style="margin-top: 20px" v-model="searchQuery" placeholder="Поиск..."></my-input>
    <div class="app__btns">
      <my-button @click="showDialog">Создать пост</my-button>
      <my-select :options="sortOptions" v-model="selectedSort"></my-select>
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"></post-form>
    </my-dialog>
    <post-list
        @remove="removePost"
        :posts="sortedAndSearchedPosts"
        v-if="!isPostLoading"
    >
    </post-list>
    <div v-else>Идет загрузка...</div>
    <my-pagination :totalPages="totalPages" v-model:currentPage="page"/>
  </div>
</template>

<script>
  import PostForm from '@/components/PostForm';
  import PostList from '@/components/PostList';
  import axios from 'axios';

  export default {
    name: 'App',
    components: {
      PostForm, PostList
    },
    data() {
      return {
        posts: [],
        dialogVisible: false,
        isPostLoading: false,
        selectedSort: '',
        searchQuery: '',
        sortOptions: [
          { value: 'title', name: 'По названию' },
          { value: 'body', name: 'По описанию' }
        ],
        page: 1,
        limit: 10,
        totalPages: 0
      }
    },
    methods: {
      createPost(post) {
        this.posts.push(post);
        this.dialogVisible = false;
      },
      removePost(post) {
        this.posts = this.posts.filter(p => p.id !== post.id);
      },
      showDialog() {
        this.dialogVisible = true;
      },
      changePage(pageNumber) {
        this.page = pageNumber;
      },
      async fetchPosts() {
        try {
            this.isPostLoading = true;
            const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
              params: {
                _page: this.page,
                _limit: this.limit
              }
            });
            this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
            this.posts = response.data;
        } catch (e) {
          alert('Ошибка');
        } finally {
          this.isPostLoading = false;
        }
      }
    },
    computed: {
      sortedPosts() {
        return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
      },
      sortedAndSearchedPosts() {
        return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
      }
    },
    watch: {
      // selectedSort(newValue) {
      //   this.posts.sort((post1, post2) => {
      //     return post1[newValue]?.localeCompare(post2[newValue]);
      //   });
      // }
      page() {
        this.fetchPosts();
        this.selectedSort = '';
      }
    },
    mounted() {
      this.fetchPosts();
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Open Sans", "Circular Std", "PT Sans Narrow", sans-serif;
  }
  button, input, select {
    font-family: "Open Sans", "Circular Std", "PT Sans Narrow", sans-serif;
    font-weight: 500;
    font-size: 16px;
  }
  .app {
    margin: 0 auto;
    max-width: 1024px;
    padding: 20px;
    overflow: hidden;
  }
  .app__btns {
    display: flex;
    justify-content: space-between;
    margin: 20px 0;
  }
</style>