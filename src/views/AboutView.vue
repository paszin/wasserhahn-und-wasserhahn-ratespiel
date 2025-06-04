<template>
  <div class="container">
    <div class="card">
      <h1>Über dieses Spiel</h1>
      <p>Dies ist ein einfaches Wörter-Ratespiel, erstellt mit Vue.js und Vite.</p>
      <p>Das Spiel zeigt dir ein Bild, und du musst das entsprechende Wort erraten.</p>
      <p>Funktionen:</p>
      <ul>
        <li>Bildbasiertes Wörterraten</li>
        <li>Punkteverfolgung</li>
        <li>Worttrennungen als Hinweise</li>
      </ul>
      <p class="credits">Erstellt von: Dein Name</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AboutView'
}
</script>

<style scoped>
h1 {
  color: var(--secondary-color);
  margin-bottom: 1rem;
}

p {
  margin-bottom: 1rem;
}

ul {
  list-style-position: inside;
  margin-bottom: 1rem;
  text-align: left;
  padding-left: 1rem;
}

.credits {
  font-style: italic;
  margin-top: 2rem;
}
</style>