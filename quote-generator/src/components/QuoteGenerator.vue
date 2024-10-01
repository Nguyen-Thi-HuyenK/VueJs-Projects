<template>
    <div class="container">
      <div class="boxSize">
        <h1 class="quoteText">{{ quote }}</h1>
        <p class="author">{{ author }}</p>
        <hr />
        <div class="quoteBtn">
          <button
            class="copyButton"
            @click="copyToClipboard"
            :disabled="copied"
          >
            {{ copied ? 'Copied!' : 'Copy' }}
          </button>
          <button class="generateQuote_next"
                  @click="generateQuote">
            Next quote
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        quote: '',
        author: '',
        copied: false
      };
    },
    methods: {
      async generateQuote() {
        try {
          const response = await fetch('https://type.fit/api/quotes');
          const quoteList = await response.json();
          const randomIdx = Math.floor(Math.random() * quoteList.length);
          const quoteText = quoteList[randomIdx].text;
          const auth = quoteList[randomIdx].author || 'Anonymous';
  
          this.quote = quoteText;
          this.author = '~ ' + auth;
        } catch (error) {
          console.error('Error fetching quote:', error);
        }
      },
      copyToClipboard() {
        const textArea = document.createElement('textarea');
        textArea.value = this.quote;
        document.body.appendChild(textArea);
        textArea.select();
        document.execCommand('copy');
        document.body.removeChild(textArea);
        this.copied = true;
  
        setTimeout(() => (this.copied = false), 2000);
      }
    },
    mounted() {
      this.generateQuote();
    }
  };
  </script>
  <style src="./styles.css" scoped></style>