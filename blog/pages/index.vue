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
  <div id="index">

    <div class="top-bar">
      <h1>Blog Posts</h1>

      <div class="drop">
        <label for="category">Filter by Category:</label>
        <select v-model="selectedCategory" @change="fetchPosts">
          <option value="">All</option>
          <option v-for="category in categories" :key="category" :value="category">
            {{ category }}
          </option>
        </select>
      </div>
    </div>

    <div v-if="posts.length" id="i-cards">
      <PostCard
        v-for="post in posts"
        :key="post.id"
        :post="post"
      />
    </div>

    <p v-else>No posts found.</p>

  </div>
</template>

<style scoped>
h1 {
  margin: 10px 0;
  font-weight: 500;
  font-size: 200%;
  color: #252627; 
}

.top-bar {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
}

.drop {
  margin: 5px;
}

label {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 400;
  color: #252627; 
  margin-right: 8px;
}

select {
  height: 30px;
  width: 150px;
  font-family: "Source Sans 3", sans-serif;
  color: #252627;
  background-color: #F1F1F1; 
  border: 1px solid #183446; 
  border-radius: 4px;
  padding: 5px;
  transition: border-color 0.2s ease; 
}

select:focus {
  outline: none;
  border-color: #8D6B94; 
}

#i-cards {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  justify-content: center;
  align-content: center;
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