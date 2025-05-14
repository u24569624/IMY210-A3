<!-- Chloe Kruger, u24569624 -->
 <script>
import PostCard from '~/components/PostCard.vue';

export default {
  components: {
    PostCard,
  },
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
          `${this.$config.STRAPI_URL}/api/posts?filters[$or][0][title][$containsi]=${encodeURIComponent(this.searchQuery)}&filters[$or][1][author][$containsi]=${encodeURIComponent(this.searchQuery)}`
        );
        const data = await response.json();
        this.posts = data.data || [];
      } catch (error) {
        console.error('Error searching posts:', error);
        this.posts = [];
      }
    },
  },
};
</script>

<template>
  <div>
    <div id="top">
      <h1>Search Posts</h1>

      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search by author or title"
        @input="searchPosts"
      />
    </div>

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

<style scoped>
#top{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
}

h1 {
  font-family: "Bitter", serif;
  font-weight: 500;
  font-size: 200%;
  color: #252627; 
  margin: 10px 0;
}

input {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 300;
  color: #252627; 
  background-color: #F1F1F1; 
  border: 1px solid #183446; 
  border-radius: 4px;
  padding: 8px 12px;
  width: 300px;
  margin: 10px 0;
  transition: border-color 0.2s ease; 
}

input:focus {
  outline: none;
  border-color: #8D6B94;
}

input::placeholder {
  color: rgba(37, 38, 39, 0.6);
}

p {
  font-family: "Source Sans 3", sans-serif;
  font-optical-sizing: auto;
  font-weight: 300;
  font-style: normal;
  color: #252627; 
  text-align: center;
  margin: 20px 0;
}
</style>