<template>
  <button class="board__btn" @click="resetBoard">Начать заново</button>
</template>

<script>
export default {
  methods: {
    resetBoard() {
      this.$emit("resetBoard");
    },
  },
};
</script>

<style scoped lang="scss">
.board__btn {
  display: block;
  margin: 0 auto;
}
</style>
