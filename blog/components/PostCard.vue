<!-- Chloe Kruger, u24569624 -->
<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
      default: () => ({}),
    },
  },
};
</script>

<template>
  <div v-if="post && post.documentId" class="post-card">
    <h2>{{ post.title }}</h2>
    <p id="author">Author: {{ post.author }}</p>
    <p>{{ post.Snippet }}</p>
    <nuxt-link :to="`/posts/${post.documentId}`">Read More</nuxt-link>
  </div>
  <div v-else class="post-card">
    <p>Post data unavailable</p>
  </div>
</template>

<style scoped>
.post-card {
  background-color: #F1F1F1; /* Matches palette */
  border: 1px solid #183446; /* Border from palette */
  box-shadow: 0 4px 12px rgba(37, 38, 39, 0.1); /* Shadow from _id.vue */
  padding: 20px;
  margin: 10px;
  max-width: 600px;
  width: 100%;
  border-radius: 8px; /* Matches _id.vue */
  transition: box-shadow 0.2s ease, transform 0.2s ease;
}

.post-card:hover {
  box-shadow: 0 6px 16px rgba(37, 38, 39, 0.15); /* Hover shadow from _id.vue */
  transform: translateY(-4px); /* Lift effect */
}

h2 {
  font-family: "Bitter", serif;
  font-weight: 500; /* Matches _id.vue */
  font-size: 1.8rem;
  color: #242627; /* Text color from palette */
  margin: 0 0 10px;
  line-height: 1.2;
}

p {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 300;
  font-size: 1rem;
  color: #242627; /* Text color from palette */
  margin: 8px 0;
  line-height: 1.6;
}

strong {
  font-weight: 500;
  color: #242627; /* Text color from palette */
}

.post-card a {
  display: inline-block;
  font-family: "Source Sans 3", sans-serif;
  font-weight: 400;
  font-size: 1rem;
  color: #F1F1F1; /* Button text from palette */
  background-color: #568EA3; /* Button background from palette */
  padding: 8px 16px;
  border-radius: 6px; /* Matches _id.vue */
  text-decoration: none;
  margin-top: 10px;
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.post-card a:hover {
  background-color: #8D6B94; /* Button hover from palette */
  transform: translateY(-2px); /* Lift effect from _id.vue */
}

@media (max-width: 600px) {
  .post-card {
    padding: 15px;
    margin: 5px;
  }

  h2 {
    font-size: 1.5rem;
  }
}
</style>