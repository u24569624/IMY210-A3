<!--Chloe Kruger, u24569624 -->
<script>
import PostCard from '~/components/PostCard.vue';

export default {
  components: {
    PostCard,
  },
  data() {
    return {
      posts: [],
      categories: ['Books', 'Movies', 'Games'],
      selectedCategory: '',
    };
  },
  async mounted() {
    await this.fetchPosts();
  },
  methods: {
    async fetchPosts() {
      let url = `${this.$config.STRAPI_URL}/api/posts`;
      if (this.selectedCategory) {
        url += `?filters[Category][$eq]=${encodeURIComponent(this.selectedCategory)}`;
      }
      try {
        const response = await fetch(url);
        const data = await response.json();
        this.posts = data.data || [];
      } catch (error) {
        console.error('Error fetching posts:', error);
        this.posts = [];
      }
    },
  },
};
</script>

<template>
  <div>
    <h1>Blog Posts</h1>
    <label for="category">Filter by Category:</label>
    <select v-model="selectedCategory" @change="fetchPosts">
      <option value="">All</option>
      <option v-for="category in categories" :key="category" :value="category">
        {{ category }}
      </option>
    </select>
    <div v-if="posts.length">
      <PostCard
        v-for="post in posts"
        :key="post.id"
        :post="post"
      />
    </div>
    <p v-else>No posts found.</p>
  </div>
</template>

