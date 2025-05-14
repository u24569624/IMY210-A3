<!-- Chloe Kruger, u24569624 -->
<script>
export default {
  name: 'Navbar',
};
</script>

<template>
  <nav class="navbar">
    <div class="navbar-brand">
      <nuxt-link to="/">Blog</nuxt-link>
    </div>

    <ul class="navbar-links">
      <li><nuxt-link to="/">Home</nuxt-link></li>
      <li><nuxt-link to="/search">Search</nuxt-link></li>
    </ul>

  </nav>
</template>

<style scoped>
.navbar {
  background-color: #252627;
  padding: 2%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #fff;
  box-shadow: 5px 1px 20px #1a1a1a;
}

.navbar-brand a {
  font-family: "Bitter", serif;
  font-size: 200%;
  font-weight:400;
  color: #fff;
  text-decoration: none;
}

.navbar-links {
  list-style: none;
  display: flex;
  margin: 0;
  padding: 0;
}

.navbar-links li {
  margin-left: 1.5rem;
}

.navbar-links a {
  color: #fff;
  text-decoration: none;
  font-size: 115%;
  font-weight:350;
}

.navbar-links a:hover {
  color: #c1c1c1;
}
</style>