<template>
  <div class="main">
    <h1 class="main__title">Крестики-нолики</h1>
    <BoardComponent :squares="squares" @makeMove="makeMove" />
    <div class="main__wrapper">
      <ButtonComponent @resetBoard="resetBoard" />
      <StatusComponent :winner="winner" :isBoardFull="isBoardFull" />
    </div>
  </div>
</template>

<script>
import BoardComponent from "./components/BoardComponent.vue";
import ButtonComponent from "./components/ButtonComponent.vue";
import StatusComponent from "./components/StatusComponent.vue";

export default {
  name: "App",
  components: {
    BoardComponent,
    ButtonComponent,
    StatusComponent,
  },
  data() {
    return {
      currentPlayer: "X",
      squares: Array(9).fill(""),
      winner: null,
      isBoardFull: false,
      moves: 0,
      winningCombos: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ],
    };
  },
  methods: {
    makeMove(index) {
      if (!this.squares[index] && !this.winner) {
        this.squares.splice(index, 1, this.currentPlayer);
        this.moves++;
        this.checkForWinner();
        this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
      }
    },
    checkForWinner() {
      for (let combo of this.winningCombos) {
        let [a, b, c] = combo;
        if (
          this.squares[a] &&
          this.squares[a] === this.squares[b] &&
          this.squares[a] === this.squares[c]
        ) {
          this.winner = this.currentPlayer;
          break;
        }
      }

      if (!this.winner && this.moves === 9) {
        this.isBoardFull = true;
      }
    },
    resetBoard() {
      this.currentPlayer = "X";
      this.squares = Array(9).fill("");
      this.winner = null;
      this.isBoardFull = false;
      this.moves = 0;
    },
  },
};
</script>

<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.main {
  &__title {
    text-align: center;
  }

  &__wrapper {
    margin: 0 auto;
    margin-top: 20px;
    width: 320px;
  }
}
</style>
