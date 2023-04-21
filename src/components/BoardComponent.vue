<template>
  <div class="board">
    <ul class="board__list">
      <li
        class="board__item"
        v-for="(square, index) in squares"
        :key="index"
        @click="makeMove(index)"
      >
        {{ square }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    squares: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    makeMove(index) {
      this.$emit("makeMove", index);
    },
  },
};
</script>

<style scoped lang="scss">
.board {
  &__list {
    display: flex;
    flex-wrap: wrap;
    width: 320px;
    margin: 0 auto;
    padding: 0;
  }

  &__item {
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
