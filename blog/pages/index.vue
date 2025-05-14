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
  <div class="index-page">
    <div class="index-content">

      <div class="top-bar">
        <h1>Blog Posts</h1>

        <div class="drop">
          <label for="category">Filter by Category:</label>

          <select v-model="selectedCategory" @change="fetchPosts" id="category">
            <option value="">All</option>
            <option v-for="category in categories" :key="category" :value="category">
              {{ category }}
            </option>
          </select>

        </div>

      </div>

      <div v-if="posts.length" class="posts-container">
        <PostCard
          v-for="post in posts"
          :key="post.id"
          :post="post"
        />
      </div>

      <p v-else>No posts found.</p>

    </div>
  </div>
</template>

<style scoped>
.index-page {
  background-color: #F1F1F1;
  min-height: 80vh;
  min-width: 90vw;
  display: flex;
  justify-content: center;
}

.index-content {
  max-width: 900%;
}

.top-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  margin-bottom: 20px;
  border-bottom: 1px solid rgba(37, 38, 39, 0.2); /* Divider from _id.vue */
}

h1 {
  font-family: "Bitter", serif;
  font-weight: 500; /* Matches _id.vue */
  font-size: 2.5rem;
  color: #242627; /* Text color from palette */
  margin: 0;
  line-height: 1.2;
}

.drop {
  display: flex;
  align-items: center;
}

label {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 400;
  font-size: 1rem;
  color: #242627; /* Text color from palette */
  margin-right: 10px;
}

select {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 300;
  font-size: 1rem;
  color: #242627; /* Text color from palette */
  background-color: #F1F1F1; /* Page background */
  border: 1px solid #183446; /* Border from palette */
  border-radius: 6px; /* Matches _id.vue */
  padding: 8px 12px;
  width: 150px;
  box-shadow: 0 2px 4px rgba(37, 38, 39, 0.1);
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

select:focus {
  outline: none;
  border-color: #8D6B94; /* Link color from palette */
  box-shadow: 0 2px 8px rgba(141, 107, 148, 0.2); /* Subtle #8D6B94 shadow */
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
  .index-content {
    padding: 15px;
  }

  h1 {
    font-size: 2rem;
  }

  .top-bar {
    flex-direction: column;
    align-items: flex-start;
  }

  .drop {
    margin-top: 10px;
  }

  select {
    width: 100%;
    max-width: 200px;
  }
}
</style>