<template>
  <div id="app">
    <div class="header">

    </div>
    <div class="game">
      <div class="row" v-for="(rows, index) in attempts" :key="index">
        <div class="slot" v-for="(slots, index) in attempts[0]" :key="index"></div>
      </div>
    </div>
    <div class="keyboard">
      <SimpleKeyboard @onKeyPress="onKeyPress"/>
    </div>
  </div>
</template>

<script>
import SimpleKeyboard from "./components/SimpleKeyboard.vue";

export default {
  name: 'App',
  components: {
    SimpleKeyboard,
  },

  data() {
    return {
      word: [],
      wordList: ["PIANO", "CODEX", "LASER", "SERVE", "CHESS", "RIGHT", "MONEY", "WATER", "NYMPH", "MAJOR", "TRUCK"],
      attempts: [['', '', '', '', ''], ['', '', '', '', ''], ['', '', '', '', ''], ['', '', '', '', ''], ['', '', '', '', ''], ['', '', '', '', '']], //6 words, 5 letters each.
      attemptsMade: 0,
      letters: 0,
    }
  },

  created() {
    var index = Math.floor(Math.random() * this.wordList.length);
    this.word = Array.from(this.wordList[index]);
  },

  methods: {
    onKeyPress(button) {

      var slots = document.querySelectorAll(".slot");

      if (button === "{bksp}") {
        this.letters = 0;
        this.attempts[this.attemptsMade] = ['', '', '', '', ''];
        for (var i = this.attemptsMade * 5; i < this.attemptsMade * 5 + 5; i++) {
          slots[i].innerText = '';
        }
        return;
      }

      if (this.letters == 5) {
        if (button === "{bksp}") {
          this.letters = 0;
          this.attempts[this.attemptsMade] = ['', '', '', '', ''];
          return;
        }

        if (button === "{enter}" ) {
          this.letters = 0;
          this.checkWord();
          this.attemptsMade++;
          return; 
        }

        return;
      }

      if (button.length > 1 || button.toUpperCase() == button) {
        return;
      }

      this.attempts[this.attemptsMade][this.letters] = button.toUpperCase();
      slots[(this.attemptsMade * 5) + this.letters].innerText = button.toUpperCase();
      this.letters++;
    },

    checkWord() {

      var slots = document.querySelectorAll(".slot");

      this.attempts[this.attemptsMade].forEach((letter, index) => {
        if (this.word.includes(letter)) {
          if (this.word[index] == letter) {
            console.log('green')
            slots[this.attemptsMade * 5 + index].style.backgroundColor = "green";
          } else {
            console.log('yellow')
            slots[this.attemptsMade * 5 + index].style.backgroundColor = "#ffcb1f";
          }
        }
      });
    }
  },
}
</script>

<style lang="scss">

@import './style/app.scss';

.header {
  width: 100%;
  background-color: #ccc;
  height: 60px;
}

.game {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
  width: 30%;
  height: 55%;
  min-width: 320px;
  border: 1px solid black;

  .row {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
    width: 95%;
    height: calc(100%/6 - 10px);
    min-height: 50px;
    border: 1px solid black;

    .slot {
      width: calc(100%/5 - 5px);
      height: 90%;
      border: 1px solid black;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 20px;
      color: #fff;
      background-color: #444;
    }
  }
}

.keyboard {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 40%;
  flex-grow: 1;
}

</style>
