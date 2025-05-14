<!-- blog/pages/search.vue -->
<!-- Chloe [Your Surname], u24569624 -->
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