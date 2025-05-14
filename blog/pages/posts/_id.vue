<!-- blog/pages/posts/_id.vue -->
<!-- Chloe [Your Surname], u24569624 -->
<template>
  <div class="post-content">
    <h1 v-if="post">{{ post.title }}</h1>
    <h1 v-else-if="!error">Loading...</h1>
    <h1 v-else>Post Not Found</h1>
    <p v-if="post"><strong>Author:</strong> {{ post.author }}</p>
    <p v-if="post"><strong>Category:</strong> {{ post.Category }}</p>
    <div v-if="post" v-html="renderedContent"></div>
    <p v-if="error">{{ error }}</p>
  </div>
</template>

<script>
import { marked } from 'marked';

export default {
  data() {
    return {
      post: null,
      error: null,
    };
  },
  async mounted() {
    const documentId = this.$route.params.id;
    if (!documentId || documentId === 'undefined') {
      this.error = 'Invalid post ID';
      return;
    }
    await this.fetchPost();
  },
  computed: {
    renderedContent() {
      if (!this.post?.Content) return '';
      const contentText = this.post.Content.map(block =>
        block.children.map(child => child.text).join('')
      ).join('\n');
      return marked(contentText);
    },
  },
  methods: {
    async fetchPost() {
      try {
        const response = await fetch(
          `${this.$config.STRAPI_URL}/api/posts/${this.$route.params.id}`
        );
        const data = await response.json();
        if (data.data) {
          this.post = data.data;
          this.error = null;
        } else {
          this.error = `Error: ${data.error?.message || 'Post not found'}`;
          this.post = null;
        }
      } catch (error) {
        this.error = 'Failed to fetch post';
        this.post = null;
      }
    },
  },
};
</script>