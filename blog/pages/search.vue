<!-- pages/search.vue -->
<template>
  <div>
    <h1>Search Posts</h1>
    <input
      v-model="searchQuery"
      type="text"
      placeholder="Search by author or title"
      @input="searchPosts"
    />
    <div v-if="posts.length">
      <PostCard
        v-for="post in posts"
        :key="post.id"
        :post="post"
      />
    </div>
    <p v-else>No results found.</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: [],
      searchQuery: '',
    };
  },
  methods: {
    async searchPosts() {
      if (!this.searchQuery) {
        this.posts = [];
        return;
      }
      try {
        const response = await fetch(
          `http://localhost:1337/api/posts?filters[$or][0][title][$containsi]=${this.searchQuery}&filters[$or][1][author][$containsi]=${this.searchQuery}`
        );
        const data = await response.json();
        this.posts = data.data;
      } catch (error) {
        console.error('Error searching posts:', error);
      }
    },
  },
};
</script>