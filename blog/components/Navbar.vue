<template>
  <nav class="navbar">
    <nuxt-link to="/" class="nav-brand">Blog</nuxt-link>
    <div class="nav-links">
      <nuxt-link to="/">Home</nuxt-link>
      <nuxt-link to="/search">Search</nuxt-link>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  background-color: #252627; 
  padding: 15px 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 4px 12px rgba(37, 38, 39, 0.1); 
  border-bottom: 1px solid #183446; 
}

.nav-brand {
  font-family: "Bitter", serif;
  font-weight: 500;
  font-size: 1.8rem;
  color: #F1F1F1; 
  text-decoration: none;
  transition: color 0.2s ease;
}

.nav-brand:hover {
  color: #8D6B94; 
}

.nav-links a {
  font-family: "Source Sans 3", sans-serif;
  font-weight: 400;
  font-size: 1rem;
  color: #F1F1F1; 
  text-decoration: none;
  margin-left: 20px;
  padding: 8px 16px;
  border-radius: 6px;
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.nav-links a:hover {
  background-color: #8D6B94; 
  transform: translateY(-2px);
}

@media (max-width: 600px) {
  .navbar {
    flex-direction: column;
    padding: 15px;
  }

  .nav-links {
    margin-top: 10px;
  }

  .nav-links a {
    margin: 5px;
  }
}
</style>