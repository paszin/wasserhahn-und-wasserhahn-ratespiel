<template>
  <div class="container">
    <div class="card">
      <h1>Errate das Wort</h1>
      
      <div v-if="words.length === 0" class="game-container">
        <div class="feedback correct">
          Glückwunsch! Du hast alle Wörter erraten!
        </div>
        <button @click="resetGame" class="btn">Spiel neu starten</button>
      </div>
      
      <div v-else-if="currentWord" class="game-container">
        <div class="image-container">
          <img :src="currentWord.imageUrl" alt="Guess this word" class="game-image" />
        </div>
        
        <div class="guess-container">
          <div class="letter-inputs">
            <input
              v-for="(letter, index) in letterInputs"
              :key="index"
              type="text"
              v-model="letterInputs[index]"
              maxlength="1"
              :disabled="disabledInputs[index]"
              :class="['letter-input', { 'letter-input-disabled': disabledInputs[index] }]"
              @input="handleLetterInput(index)"
              @keydown.delete="handleDelete(index)"
              @keydown.left="focusPrevInput(index)"
              @keydown.right="focusNextInput(index)"
              @keydown.enter="isLastInput(index) && checkGuess()"
              ref="letterInputs"
            />
          </div>
          <div class="button-group">
            <button @click="checkGuess" class="btn">Antwort prüfen</button>
            <button @click="skipWord" class="btn btn-secondary">Überspringen</button>
            <button @click="showHint" class="btn btn-info">Tipp</button>
          </div>
        </div>
        
        <div v-if="feedback" class="feedback" :class="{ correct: isCorrect }">
          {{ feedback }}
        </div>
        
        <div class="score">
          Erratene Worte: {{ score }} / {{ totalWords }}
          <div class="hints-used">Benutzte Tipps: {{ hintsUsed }}</div>
        </div>
      </div>
      
      <div v-else class="loading">
        Spiel wird geladen...
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GameView',
  data() {
    return {
      allWords: [
        // Sehr häufige/bekannte Wörter
        {
          word: 'wasserhahn',
          imageUrl: '/src/assets/images/wasserhahn.jpg',
          splitIndex: 6,
          solved: false
        },
        {
          word: 'glühbirne',
          imageUrl: '/src/assets/images/glühbirne.jpg',
          splitIndex: 4,
          solved: false
        },
        {
          word: 'zahnrad',
          imageUrl: '/src/assets/images/zahnrad.jpg',
          splitIndex: 4,
          solved: false
        },
        {
          word: 'schlafmütze',
          imageUrl: '/src/assets/images/schlafmütze.jpg',
          splitIndex: 6,
          solved: false
        },
        {
          word: 'augapfel',
          imageUrl: '/src/assets/images/augapfel.jpg',
          splitIndex: 3,
          solved: false
        },
        {
          word: 'ohrwurm',
          imageUrl: '/src/assets/images/ohrwurm.jpg',
          splitIndex: 3,
          solved: false
        },
        {
          word: 'brillenschlange',
          imageUrl: '/src/assets/images/brillenschlange.jpg',
          splitIndex: 7,
          solved: false
        },
        
        {
          word: 'schlüsselring',
          imageUrl: '/src/assets/images/schlüsselring.jpg',
          splitIndex: 9,
          solved: false
        },
        {
          word: 'wolkenkratzer',
          imageUrl: '/src/assets/images/wolkenkratzer.jpg',
          splitIndex: 6,
          solved: false
        },
        // Mittelhäufige Wörter
        {
          word: 'stuhlgang',
          imageUrl: '/src/assets/images/stuhlgang.jpg',
          splitIndex: 5,
          solved: false
        },
        {
          word: 'eselsbrücke',
          imageUrl: '/src/assets/images/eselsbrücke.jpg',
          splitIndex: 5,
          solved: false
        },
        {
          word: 'kopfkino',
          imageUrl: '/src/assets/images/kopfkino.jpg',
          splitIndex: 4,
          solved: false
        },
        {
          word: 'armleuchter',
          imageUrl: '/src/assets/images/armleuchter.jpg',
          splitIndex: 3,
          solved: false
        },
        {
          word: 'quadratwurzel',
          imageUrl: '/src/assets/images/quadratwurzel.jpg',
          splitIndex: 8,
          solved: false
        },
        {
          word: 'schlüsselbein',
          imageUrl: '/src/assets/images/schlüsselbein.jpg',
          splitIndex: 9,
          solved: false
        },
        // Weniger häufige Wörter
        {
          word: 'torjäger',
          imageUrl: '/src/assets/images/torjäger.jpg',
          splitIndex: 3,
          solved: false
        },
        {
          word: 'katzenwäsche',
          imageUrl: '/src/assets/images/katzenwäsche.jpg',
          splitIndex: 6,
          solved: false
        },
        {
          word: 'drahtzieher',
          imageUrl: '/src/assets/images/drahtzieher.jpg',
          splitIndex: 5,
          solved: false
        },
        {
          word: 'bildschirm',
          imageUrl: '/src/assets/images/bildschirm.jpg',
          splitIndex: 4,
          solved: false
        },
        {
          word: 'eispickel',
          imageUrl: '/src/assets/images/eispickel.jpg',
          splitIndex: 3,
          solved: false
        },
        {
          word: 'martinshorn',
          imageUrl: '/src/assets/images/martinshorn.jpg',
          splitIndex: 7,
          solved: false
        },
      ],
      words: [],
      currentWordIndex: 0,
      letterInputs: [],
      disabledInputs: [],
      feedback: '',
      isCorrect: false,
      score: 0,
      hintsUsed: 0
    }
  },
  computed: {
    currentWord() {
      return this.words[this.currentWordIndex];
    },
    userGuess() {
      return this.letterInputs.join('');
    },
    totalWords() {
      return this.allWords.length;
    }
  },
  created() {
    this.filterUnsolvedWords();
    this.resetLetterInputs();
  },
  methods: {
    checkGuess() {
      if (!this.userGuess.trim()) return;
      
      if (this.userGuess.toLowerCase() === this.currentWord.word.toLowerCase()) {
        this.feedback = 'Richtig! Gut gemacht!';
        this.isCorrect = true;
        this.score += 1;
        
        // Mark the word as solved
        const wordIndex = this.allWords.findIndex(w => w.word === this.currentWord.word);
        if (wordIndex !== -1) {
          this.allWords[wordIndex].solved = true;
        }
        
        // Move to next word after a delay
        setTimeout(() => {
          this.filterUnsolvedWords();
          this.nextWord();
        }, 1500);
      } else {
        this.feedback = 'Falsch. Versuche es nochmal!';
        this.isCorrect = false;
      }
      
      // Clear inputs
      this.resetLetterInputs();
    },
    nextWord() {
      if (this.words.length === 0) {
        this.feedback = 'Glückwunsch! Du hast alle Wörter erraten!';
        this.isCorrect = true;
        return;
      }
      
      this.currentWordIndex = (this.currentWordIndex + 1) % this.words.length;
      this.feedback = '';
      this.resetLetterInputs();
    },
    skipWord() {
      this.nextWord();
    },
    
    resetLetterInputs() {
      this.letterInputs = Array(this.currentWord.word.length).fill('');
      this.disabledInputs = Array(this.currentWord.word.length).fill(false);
      this.$nextTick(() => {
        if (this.$refs.letterInputs && this.$refs.letterInputs.length > 0) {
          this.$refs.letterInputs[0].focus();
        }
      });
    },
    
    handleLetterInput(index) {
      if (this.letterInputs[index]) {
        // Convert to uppercase
        this.letterInputs[index] = this.letterInputs[index].toUpperCase();
        
        // Move to next input if available
        this.focusNextInput(index);
        
        // For mobile keyboards (especially Android), add a small delay to ensure the input event completes
        setTimeout(() => {
          this.focusNextInput(index);
        }, 10);
      }
    },
    
    handleDelete(index) {
      if (!this.letterInputs[index] && index > 0) {
        // If current input is empty and delete is pressed, focus previous input
        this.focusPrevInput(index);
      }
    },
    
    focusPrevInput(index) {
      if (index > 0) {
        this.$nextTick(() => {
          this.$refs.letterInputs[index - 1].focus();
        });
      }
    },
    
    focusNextInput(index) {
      if (index < this.currentWord.word.length - 1) {
        this.$nextTick(() => {
          this.$refs.letterInputs[index + 1].focus();
        });
      }
    },
    
    isLastInput(index) {
      return index === this.currentWord.word.length - 1;
    },
    
    filterUnsolvedWords() {
      this.words = this.allWords.filter(word => !word.solved);
      
      // Reset index if it's out of bounds
      if (this.currentWordIndex >= this.words.length) {
        this.currentWordIndex = 0;
      }
    },
    
    resetGame() {
      // Reset all words to unsolved
      this.allWords.forEach(word => {
        word.solved = false;
      });
      
      this.filterUnsolvedWords();
      this.score = 0;
      this.hintsUsed = 0;
      this.feedback = '';
      this.resetLetterInputs();
    },
    
    showHint() {
      // Check if first letter is already shown
      const firstLetterShown = this.disabledInputs[0] && 
        this.letterInputs[0].toLowerCase() === this.currentWord.word[0].toLowerCase();
      
      // Check if split index letter is already shown
      const splitLetterShown = this.disabledInputs[this.currentWord.splitIndex] && 
        this.letterInputs[this.currentWord.splitIndex].toLowerCase() === this.currentWord.word[this.currentWord.splitIndex].toLowerCase();
      
      // Determine which hint to show
      let hintIndex;
      if (firstLetterShown && !splitLetterShown) {
        // First letter already shown, show split letter
        hintIndex = this.currentWord.splitIndex;
      } else if (!firstLetterShown && splitLetterShown) {
        // Split letter already shown, show first letter
        hintIndex = 0;
      } else {
        // Neither or both are shown, choose randomly
        hintIndex = Math.random() < 0.5 ? 0 : this.currentWord.splitIndex;
      }
      
      // Set the letter in the input field
      this.letterInputs[hintIndex] = this.currentWord.word[hintIndex].toUpperCase();
      
      // Disable the input field
      this.disabledInputs[hintIndex] = true;
      
      // Increment hints used counter
      this.hintsUsed++;
      
      // Focus the next empty input after the hint
      let nextEmptyIndex = -1;
      for (let i = 0; i < this.letterInputs.length; i++) {
        if (!this.letterInputs[i] && i !== hintIndex) {
          nextEmptyIndex = i;
          break;
        }
      }
      
      if (nextEmptyIndex !== -1) {
        this.$nextTick(() => {
          this.$refs.letterInputs[nextEmptyIndex].focus();
        });
      }
    }
  }
}
</script>

<style scoped>
.game-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
}

.image-container {
  width: 100%;
  max-width: 300px;
  margin: 0 auto;
}

.game-image {
  width: 100%;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.guess-container {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.letter-inputs {
  display: flex;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
  width: 100%;
  max-width: 400px;
  flex-wrap: wrap;
}

.letter-input {
  width: 2rem;
  height: 2.5rem;
  text-align: center;
  font-size: 1.5rem;
  border: 2px solid #ccc;
  border-radius: 4px;
  text-transform: uppercase;
}

@media (max-width: 480px) {
  .letter-input {
    width: 1.5rem;
    height: 2rem;
    font-size: 1.2rem;
  }
}

.letter-input-disabled {
  background-color: #e9ecef;
  color: #495057;
  font-weight: bold;
}

.feedback {
  padding: 0.75rem;
  border-radius: 4px;
  background-color: #f8d7da;
  color: #721c24;
  width: 100%;
  max-width: 300px;
  text-align: center;
}

.feedback.correct {
  background-color: #d4edda;
  color: #155724;
}

.score {
  font-weight: bold;
  font-size: 1.2rem;
  margin-top: 1rem;
}

.hints-used {
  font-size: 0.9rem;
  color: #6c757d;
  margin-top: 0.3rem;
  font-weight: normal;
}

.button-group {
  display: flex;
  gap: 0.5rem;
  margin-top: 0.5rem;
  flex-wrap: wrap;
  justify-content: center;
}

.btn-secondary {
  background-color: #6c757d;
}

.btn-info {
  background-color: #17a2b8;
}
</style>