<!-- pages/posts/_id.vue -->
<!-- pages/posts/_id.vue -->
<template>
  <div class="post-content">
    <h1>{{ post?.attributes.title }}</h1>
    <p><strong>Author:</strong> {{ post?.attributes.author }}</p>
    <p><strong>Category:</strong> {{ post?.attributes.category }}</p>
    <div v-html="renderedContent"></div>
    <nuxt-link to="/">Back to Home</nuxt-link>
  </div>
</template>
<!-- Script remains the same -->

<script>
import { marked } from 'marked';

export default {
  data() {
    return {
      post: null,
    };
  },
  async mounted() {
    await this.fetchPost();
  },
  computed: {
    renderedContent() {
      return this.post?.attributes.content
        ? marked(this.post.attributes.content)
        : '';
    },
  },
  methods: {
    async fetchPost() {
      try {
        const response = await fetch(
          `http://localhost:1337/api/posts/${this.$route.params.id}`
        );
        const data = await response.json();
        this.post = data.data;
      } catch (error) {
        console.error('Error fetching post:', error);
      }
    },
  },
};
</script>