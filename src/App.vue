<template>
  <header>
    <nav>
      <router-link to="/">Anleitung</router-link> |
      <router-link to="/game">Spiel</router-link>
      <!-- <router-link to="/about">Über</router-link> -->
    </nav>
  </header>
  <main>
    <router-view />
  </main>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
}

header {
  line-height: 1.5;
}

nav {
  padding: 1rem 0;
  margin-bottom: 2rem;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
  margin: 0 0.5rem;
  text-decoration: none;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>