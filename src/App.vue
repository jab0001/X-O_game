<template>
  <div class="board">
    <h1 class="board__title">Крестики-нолики</h1>
    <input
      type="number"
      v-model="boardSize"
      @input="resetBoard"
      min="3"
      max="10"
    />
    <ul class="board__list" :style="{ width: boardSize * 60 + 'px' }">
      <li
        class="board__item"
        v-for="(square, index) in squares"
        :key="index"
        @click="makeMove(index)"
      >
        {{ square }}
      </li>
    </ul>
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
      boardSize: 3,
      squares: [],
      winner: null,
      isBoardFull: false,
      moves: 0,
    };
  },
  watch: {
    boardSize: function () {
      this.resetBoard();
    },
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
      for (let i = 0; i < this.boardSize; i++) {
        for (let j = 0; j <= this.boardSize - 3; j++) {
          if (
            this.squares[i * this.boardSize + j] &&
            this.squares[i * this.boardSize + j] ===
              this.squares[i * this.boardSize + j + 1] &&
            this.squares[i * this.boardSize + j] ===
              this.squares[i * this.boardSize + j + 2]
          ) {
            this.winner = this.currentPlayer;
            return;
          }
        }
      }

      for (let i = 0; i <= this.boardSize - 3; i++) {
        for (let j = 0; j < this.boardSize; j++) {
          if (
            this.squares[i * this.boardSize + j] &&
            this.squares[i * this.boardSize + j] ===
              this.squares[(i + 1) * this.boardSize + j] &&
            this.squares[i * this.boardSize + j] ===
              this.squares[(i + 2) * this.boardSize + j]
          ) {
            this.winner = this.currentPlayer;
            return;
          }
        }
      }

      for (let i = 0; i <= this.boardSize - 3; i++) {
        for (let j = 0; j <= this.boardSize - 3; j++) {
          if (
            this.squares[i * this.boardSize + j] &&
            this.squares[i * this.boardSize + j] ===
              this.squares[(i + 1) * this.boardSize + j + 1] &&
            this.squares[i * this.boardSize + j] ===
              this.squares[(i + 2) * this.boardSize + j + 2]
          ) {
            this.winner = this.currentPlayer;
            return;
          }
          if (
            this.squares[i * this.boardSize + (this.boardSize - j - 1)] &&
            this.squares[i * this.boardSize + (this.boardSize - j - 1)] ===
              this.squares[
                (i + 1) * this.boardSize + (this.boardSize - j - 2)
              ] &&
            this.squares[i * this.boardSize + (this.boardSize - j - 1)] ===
              this.squares[(i + 2) * this.boardSize + (this.boardSize - j - 3)]
          ) {
            this.winner = this.currentPlayer;
            return;
          }
        }
      }

      if (!this.winner && this.moves === this.boardSize * this.boardSize) {
        this.isBoardFull = true;
      }
    },
    resetBoard() {
      this.currentPlayer = "X";
      this.squares = Array(this.boardSize * this.boardSize).fill("");
      this.winner = null;
      this.isBoardFull = false;
      this.moves = 0;
    },
  },
  created() {
    this.resetBoard();
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

  &__list {
    display: flex;
    flex-wrap: wrap;
    margin: 0 auto;
    padding: 0;
  }

  &__wrapper {
    margin: 0 auto;
    margin-top: 20px;
    width: 100%;
  }

  &__btn {
    display: block;
    margin: 0 auto;
  }

  &__status {
    margin-top: 10px;
    text-align: center;
  }

  &__item {
    border: 1px solid #ccc;
    width: 60px;
    height: 60px;
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
