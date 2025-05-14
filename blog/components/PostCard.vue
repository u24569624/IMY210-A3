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
p {
  font-family: "Source Sans 3", sans-serif;
  font-optical-sizing: auto;
  font-weight: 100;
  font-style: normal;
  color: #252627; 
  margin: 0.5em 0;
}

h2 {
  font-family: "Bitter", serif;
  font-optical-sizing: auto;
  font-weight: 300;
  font-style: normal;
  font-size: 180%;
  margin: 0.5%;
  color: #252627; 
}

#author{
  color: #8f8f8f;
}

.post-card {
  background-color: #e9e9e9; 
  border: 1px solid #1834467b; 
  border-radius: 8px;
  margin: 1%;
  width: 90%;
  min-height: 200px;
  padding: 1%;
  box-shadow: 0 2px 8px rgba(37, 38, 39, 0.1); 
  transition: transform 0.2s ease, box-shadow 0.2s ease; 
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: baseline;
}

.post-card:hover {
  transform: translateY(-4px); 
  box-shadow: 0 4px 12px rgba(37, 38, 39, 0.15); 
}

.post-card a {
  color: #ffffff; 
  background-color: #8D6B94;
  text-decoration: none;
  font-family: "Source Sans 3", sans-serif;
  font-weight: 400;
  padding: 0.5em 1em;
  display: inline-block;
  width: 10%;
  border-radius: 4px;
  transition: background-color 0.2s ease, color 0.2s ease; 
}

.post-card a:hover {
  background-color: #8D6B94; 
  color: #F1F1F1;
}
</style>