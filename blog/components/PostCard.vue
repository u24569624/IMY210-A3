<!-- blog/components/PostCard.vue -->
<!-- Chloe [Your Surname], u24569624 -->
<template>
  <div v-if="post && post.documentId" class="post-card">
    <h2>{{ post.title }}</h2>
    <p><strong>Author:</strong> {{ post.author }}</p>
    <p>{{ post.Snippet }}</p>
    <nuxt-link :to="`/posts/${post.documentId}`">Read More</nuxt-link>
  </div>
  <div v-else class="post-card">
    <p>Post data unavailable</p>
  </div>
</template>

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