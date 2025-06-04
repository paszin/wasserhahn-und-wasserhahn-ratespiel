<template>
  <div class="word-card">
    <img :src="imageUrl" :alt="hint" class="word-image" />
    <p class="word-hint">{{ hint }}</p>
  </div>
</template>

<script>
export default {
  name: 'WordCard',
  props: {
    imageUrl: {
      type: String,
      required: true
    },
    hint: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
.word-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.word-image {
  width: 100%;
  max-width: 250px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.word-hint {
  font-style: italic;
  color: #666;
}
</style>