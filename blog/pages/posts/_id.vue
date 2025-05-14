<!-- Chloe Kruger, u24569624 -->
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

<style scoped>
.post-content {
  background-color: #F1F1F1; 
  padding: 20px;
  margin: 10px;
  border: 1px solid #183446; 
  box-shadow: 0 2px 8px rgba(37, 38, 39, 0.1); 
}

h1 {
  font-family: "Bitter", serif;
  font-weight: 500;
  font-size: 200%;
  color: #252627; 
  margin: 10px 0;
}

p {
  font-family: "Source Sans 3", sans-serif;
  font-optical-sizing: auto;
  font-weight: 300;
  font-style: normal;
  color: #252627; 
  margin: 10px 0;
}

strong {
  font-weight: 500;
  color: #252627; 
}

.post-content :deep(a) {
  color: #8D6B94; 
  text-decoration: none;
  transition: color 0.2s ease;
}

.post-content :deep(a:hover) {
  color: #568EA3; 
}
</style>