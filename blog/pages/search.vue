<template>
  <div class="search-page">
    <div class="search-content">
      <h1>Search Posts</h1>
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search by author or title"
        @input="searchPosts"
      />
      <div v-if="posts.length" class="posts-container">
        <PostCard
          v-for="post in posts"
          :key="post.id"
          :post="post"
        />
      </div>
      <p v-else>No results found.</p>
    </div>
  </div>
</template>

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

<style scoped>
.search-page {
  background-color: #F1F1F1; /* Page background from palette */
  min-height: 80vh;
  display: flex;
  justify-content: center;
  padding: 20px;
}

.search-content {
  max-width: 800px; /* Matches _id.vue */
  width: 100%;
  padding: 20px;
}

h1 {
  font-family: "Bitter", serif;
  font-weight: 500; /* Matches _id.vue */
  font-size: 2.5rem;
  color: #242627; /* Text color from palette */
  margin: 0 0 20px;
  line-height: 1.2;
}

input {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 300;
  font-size: 1rem;
  color: #242627; /* Text color from palette */
  background-color: #F1F1F1; /* Page background */
  border: 1px solid #183446; /* Border from palette */
  border-radius: 6px; /* Matches _id.vue */
  padding: 10px 16px;
  width: 100%;
  max-width: 400px;
  margin: 10px 0;
  box-shadow: 0 2px 4px rgba(37, 38, 39, 0.1);
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

input:focus {
  outline: none;
  border-color: #8D6B94; /* Link color from palette */
  box-shadow: 0 2px 8px rgba(141, 107, 148, 0.2); /* Subtle #8D6B94 shadow */
}

input::placeholder {
  color: rgba(37, 38, 39, 0.6); /* Semi-transparent #242627 */
}

.posts-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

p {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 300;
  font-size: 1.1rem;
  color: #242627; /* Text color from palette */
  text-align: center;
  margin: 20px 0;
  line-height: 1.6;
}

@media (max-width: 600px) {
  .search-content {
    padding: 15px;
  }

  h1 {
    font-size: 2rem;
  }

  input {
    max-width: 100%;
  }
}
</style>