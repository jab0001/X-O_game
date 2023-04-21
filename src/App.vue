<template>
  <div class="board">
    <h1 class="board__title">Крестики-нолики</h1>
    <div class="board__container">
      <div
        class="board__square"
        v-for="(square, index) in squares"
        :key="index"
        @click="makeMove(index)"
      >
        {{ square }}
      </div>
    </div>
    <div class="board__wrapper">
      <button class="board__btn" @click="resetBoard">Начать заново</button>
      <p class="board__status" v-if="winner">Победил игрок {{ winner }}</p>
      <p class="board__status" v-else-if="isBoardFull">Ничья!</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
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

.board {
  &__title {
    text-align: center;
  }

  &__container {
    display: flex;
    flex-wrap: wrap;
    width: 320px;
    margin: 0 auto;
  }

  &__wrapper {
    margin: 0 auto;
    margin-top: 20px;
    width: 320px;
  }

  &__btn {
    display: block;
    margin: 0 auto;
  }

  &__status {
    margin-top: 10px;
    text-align: center;
  }

  &__square {
    border: 1px solid #ccc;
    width: calc(100% / 3);
    height: calc(320px / 3);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 48px;
    cursor: pointer;
    box-sizing: border-box;

    &:hover {
      background-color: #eee;
    }
  }
}
</style>
