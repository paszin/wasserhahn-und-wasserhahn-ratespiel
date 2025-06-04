# Word Guessing Game

A simple image-based word guessing game built with Vue.js and Vite.

## Project Setup

```sh
npm install
```

### Development Server

```sh
npm run dev
```

### Build for Production

```sh
npm run build
```

### Preview Production Build

```sh
npm run preview
```

## Game Features

- Image-based word guessing
- Multiple game levels
- Score tracking
- Responsive design

## Project Structure

- `src/views/` - Main page components
- `src/components/` - Reusable Vue components
- `src/router/` - Vue Router configuration
- `src/assets/` - Static assets and CSS

## Adding New Words

To add new words to the game, edit the `words` array in `src/views/GameView.vue`:

```js
words: [
  {
    word: 'newword',
    imageUrl: 'path/to/image.jpg',
    hint: 'A hint for the word'
  },
  // Add more words here
]
```