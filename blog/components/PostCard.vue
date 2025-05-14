<!-- components/PostCard.vue -->
<template>
  <div class="post-card">
    <h2>{{ post.attributes.title }}</h2>
    <p><strong>Author:</strong> {{ post.attributes.author }}</p>
    <p>{{ post.attributes.snippet }}</p>
    <nuxt-link :to="`/posts/${post.id}`">Read More</nuxt-link>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
};
</script>