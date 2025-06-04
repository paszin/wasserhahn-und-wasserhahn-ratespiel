<template>
  <div class="container">
    <div class="card">
      <h1>Wasserhahn und Wasserhahn</h1>
      <p style="margin-top: -15px; font-weight: bold;">und weitere idiomatische Komposita</p>
      <h2>Willkommen beim Wörter-Ratespiel!</h2>
      <p>Versuche, das Wort anhand des gezeigten Bildes zu erraten. Die Bilder stellen die Begriffe im wörtlichen Sinne dar, wobei die Wörter jedoch normalerweise im übertragenden Sinne genutzt werden.</p>
      <p>Viel Spaß beim Raten!</p>
      <router-link to="/game" class="btn">Spiel starten</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView'
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
</style>