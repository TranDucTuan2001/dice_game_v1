<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players
        :isWinner="isWinner"
        :scorePlayer="scorePlayer"
        :activePlayer="activePlayer"
        :currentScore="currentScore"
      />

      <controls
        :isPlaying="isPlaying"
        :finalScore="finalScore"
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
        v-on:handleHoldScore="handleHoldScore"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
      />
      <dices :dices="dices" />
      <popup-rule
        :isOpenPopup="isOpenPopup"
        v-on:handleConfirm="handleConfirm"
      />
    </div>
  </div>
</template>

<script>
import Controls from "./components/Controls.vue";
import Dices from "./components/Dices.vue";
import Players from "./components/Players.vue";
import PopupRule from "./components/PopupRule.vue";
export default {
  components: { Players, Controls, Dices, PopupRule },
  name: "App",
  data() {
    return {
      isOpenPopup: false,
      isPlaying: false,
      scorePlayer: [0, 0],
      activePlayer: 0,
      currentScore: 0,
      dices: [1, 5],
      finalScore: 50,
    };
  },
  computed: {
    isWinner() {
      let { scorePlayer, finalScore, activePlayer } = this;
      if (scorePlayer[activePlayer] >= finalScore) {
        this.isPlaying = false;
        return true;
      }
      return false;
    },
  },
  methods: {
    handleChangeFinalScore(e) {
      var number = parseInt(e.target.value);

      if (isNaN(number)) {
        this.finalScore = "";
      } else {
        this.finalScore = number;
      }
    },
    handleNewGame() {
      this.isOpenPopup = true;
    },
    nextPlayer() {
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleConfirm() {
      this.isPlaying = true;
      this.isOpenPopup = false;
      this.scorePlayer = [0, 0];
      this.currentScore = 0;
      this.dices = [5, 1];
    },
    handleRollDice() {
      if (this.isPlaying) {
        var dice1 = Math.floor(Math.random() * 6) + 1;
        var dice2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [dice1, dice2];
        if (dice1 === 1 || dice2 === 1) {
          let activePlayer = this.activePlayer;
          setTimeout(() => {
            alert(
              `Người chơi ${activePlayer + 1} đã quay trúng số 1. Rất tiếc`
            );
          }, 10);
          this.nextPlayer();
        } else {
          this.currentScore += dice1 + dice2;
        }
      } else {
        alert("vui lòng nhấn new game!");
      }
    },
    handleHoldScore() {
      if (this.isPlaying) {
        let { scorePlayer, activePlayer, currentScore } = this;
        let scoreOld = scorePlayer[activePlayer];
        // let cloneScorePlayer = [...scorePlayer];
        // cloneScorePlayer[activePlayer] = scoreOld + currentScore;
        // this.scorePlayer = cloneScorePlayer;
        this.$set(this.scorePlayer, activePlayer, scoreOld + currentScore);
        if (!this.isWinner) {
          this.nextPlayer();
        }
      } else {
        alert("vui lòng nhấn new game!");
      }
    },
  },
};
</script>

<style>
/**********************************************
*** GENERAL
**********************************************/

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url("assets/back.jpg");
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}
</style>
