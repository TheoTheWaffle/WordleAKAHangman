<template>
    <span :class="feedbackClass">
      {{ letter }}
    </span>
  </template>
  
  <script>
  export default {
    props: {
      letter: String,
      feedback: String
    },
    computed: {
      feedbackClass() {
        return {
          'text-success': this.feedback === 'green',
          'text-warning': this.feedback === 'yellow',
          'text-secondary': this.feedback === ''
        };
      }
    }
  };
  </script>
  
  <style scoped>
  span {
    font-size: 24px;
    margin: 0 5px;
  }
  .text-success {
    color: green;
  }
  .text-warning {
    color: yellow;
  }
  .text-secondary {
    color: #ccc;
  }
  </style>
  