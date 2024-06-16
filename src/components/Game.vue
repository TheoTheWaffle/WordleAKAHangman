<template>
    <div class="game">
      <h1>Wordle - Hádej slovo</h1>
      <div class="word-display">
        <span v-for="(feedback, index) in feedbacks" :key="index" :class="feedbackClass(feedback)">
          {{ feedback.letter }}
        </span>
      </div>
      <div class="keyboard">
        <button v-for="letter in alphabet" :key="letter" @click="checkLetter(letter)" :disabled="usedLetters.includes(letter) || gameEnded">
          {{ letter }}
        </button>
      </div>
      <div class="attempts">
        <p>Zbývající pokusy: {{ remainingAttempts }}</p>
      </div>
      <div v-if="gameEnded">
        <p v-if="remainingAttempts === 0">Prohráli jste! Správné slovo bylo: {{ currentWord.join('') }}</p>
        <p v-else>Gratulujeme! Uhodli jste slovo!</p>
        <button @click="newGame">Hrát znovu</button>
      </div>
      <div class="difficulty">
        <p>Obtížnost:</p>
        <button @click="setDifficulty('easy')">Easy</button>
        <button @click="setDifficulty('normal')">Normal</button>
        <button @click="setDifficulty('hard')">Hard</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Game',
    data() {
      return {
        words: ['test', 'kotel', 'okno', 'ulice', 'houska'], // předem definovaný seznam slov
        currentWord: '',
        feedbacks: [],
        usedLetters: [],
        remainingAttempts: 8, // změna počtu pokusů podle obtížnosti
        gameEnded: false,
        alphabet: 'abcdefghijklmnopqrstuvwxyz'.split(''),
        difficulty: 'normal' // výchozí obtížnost
      };
    },
    created() {
      this.newGame();
    },
    methods: {
      newGame() {
        this.currentWord = this.words[Math.floor(Math.random() * this.words.length)].split('');
        this.feedbacks = new Array(this.currentWord.length).fill({ letter: '', status: '' });
        this.usedLetters = [];
        this.remainingAttempts = this.getAttemptsByDifficulty();
        this.gameEnded = false;
      },
      checkLetter(letter) {
        if (this.gameEnded || this.usedLetters.includes(letter)) return;
  
        this.usedLetters.push(letter);
        let found = false;
  
        for (let i = 0; i < this.currentWord.length; i++) {
          if (this.currentWord[i] === letter) {
            this.feedbacks[i] = { letter: letter, status: 'yellow' }; // správné písmeno, ale na špatném místě
            found = true;
          }
        }
  
        if (!found) {
          this.remainingAttempts--;
        }
  
        if (this.remainingAttempts === 0 || this.feedbacks.every(feedback => feedback.status === 'yellow')) {
          this.revealAllLetters();
          this.gameEnded = true;
        } else if (this.feedbacks.every(feedback => feedback.status === 'yellow')) {
          this.gameEnded = true;
        }
      },
      revealAllLetters() {
        for (let i = 0; i < this.currentWord.length; i++) {
          if (this.feedbacks[i].status === '') {
            this.feedbacks[i] = { letter: this.currentWord[i], status: 'yellow' };
          }
        }
      },
      feedbackClass(feedback) {
        return {
          'text-warning': feedback.status === 'yellow',
          'text-secondary': feedback.status === ''
        };
      },
      setDifficulty(difficulty) {
        this.difficulty = difficulty;
        this.newGame();
      },
      getAttemptsByDifficulty() {
        switch (this.difficulty) {
          case 'easy':
            return 10;
          case 'hard':
            return 6;
          default:
            return 8; // normal mode
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .game {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 20px;
  }
  .word-display {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
  }
  .keyboard {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 5px;
  }
  .keyboard button {
    padding: 10px 15px;
    font-size: 18px;
    background-color: lightblue;
    border: none;
    cursor: pointer;
  }
  .keyboard button[disabled] {
    background-color: #ccc;
    cursor: not-allowed;
  }
  .attempts {
    margin-top: 20px;
  }
  .difficulty {
    margin-top: 20px;
  }
  .text-warning {
    color: yellow;
  }
  .text-secondary {
    color: #ccc;
  }
  </style>
  