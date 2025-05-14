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
  <div class="post-page">
    <div class="post-content">
      <div v-if="post" class="post-header">
        <h1>{{ post.title }}</h1>
        <div class="post-meta">
          <p><strong>Author:</strong> {{ post.author }}</p>
          <p><strong>Category:</strong> <span class="category-tag">{{ post.Category }}</span></p>
        </div>
      </div>
      <div v-else-if="!error" class="loading">
        <h1>Loading...</h1>
      </div>
      <div v-else class="error">
        <h1>Post Not Found</h1>
        <p>{{ error }}</p>
      </div>
      <div v-if="post" class="post-body" v-html="renderedContent"></div>
      <nuxt-link v-if="post || error" to="/" class="back-button">Back to Home</nuxt-link>
    </div>
  </div>
</template>

<style scoped>
.post-page {
  min-height: 80vh;
  display: flex;
  justify-content: center;
  padding: 20px;
}

.post-content {
  background-color: #F1F1F1; 
  border: 1px solid #183446; 
  box-shadow: 0 4px 12px rgba(37, 38, 39, 0.1); 
  padding: 30px;
  margin: 10px;
  max-width: 800px;
  width: 100%;
  border-radius: 8px;
  transition: box-shadow 0.2s ease;
}

.post-content:hover {
  box-shadow: 0 6px 16px rgba(37, 38, 39, 0.15); 
}

.post-header {
  border-bottom: 1px solid rgba(37, 38, 39, 0.2); 
  padding-bottom: 20px;
  margin-bottom: 20px;
}

h1 {
  font-family: "Bitter", serif;
  font-weight: 500;
  font-size: 2.5rem; 
  color: #252627;
  margin: 0 0 15px;
  line-height: 1.2;
}

.post-meta p {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 300;
  font-size: 1rem;
  color: #252627;
  margin: 5px 0;
  display: inline-block;
  margin-right: 20px;
}

.post-meta strong {
  font-weight: 500;
  color: #252627;
}

.category-tag {
  background-color: #8D6B94;
  color: #F1F1F1; 
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 0.9rem;
  font-weight: 400;
}

.post-body {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 300;
  font-size: 1.1rem;
  color: #252627;
  line-height: 1.6;
  margin: 20px 0;
}

.post-body :deep(h2) {
  font-family: "Bitter", serif;
  font-weight: 500;
  font-size: 1.8rem;
  color: #252627; 
  margin: 20px 0 10px;
}

.post-body :deep(p) {
  margin: 10px 0;
}

.post-body :deep(a) {
  color: #8D6B94; 
  text-decoration: none;
  transition: color 0.2s ease;
}

.post-body :deep(a:hover) {
  color: #568EA3;
}

.loading, .error {
  text-align: center;
  padding: 40px 0;
}

.loading h1, .error h1 {
  font-size: 2rem;
}

.error p {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 300;
  color: #252627;
  margin: 10px 0;
}

.back-button {
  display: inline-block;
  font-family: "Source Sans 3", sans-serif;
  font-weight: 400;
  font-size: 1rem;
  color: #F1F1F1; 
  background-color: #568EA3; 
  padding: 10px 20px;
  border-radius: 6px;
  text-decoration: none;
  margin-top: 20px;
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.back-button:hover {
  background-color: #8D6B94; 
  transform: translateY(-2px); 
}

@media (max-width: 600px) {
  .post-content {
    padding: 20px;
    margin: 5px;
  }

  h1 {
    font-size: 2rem;
  }

  .post-meta p {
    display: block;
    margin: 8px 0;
  }
}
</style>