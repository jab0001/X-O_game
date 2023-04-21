<template>
  <p class="status" v-if="winner || isBoardFull">
    {{ winner ? `Победил ${winner}` : "Ничья!" }}
  </p>
</template>

<script>
export default {
  props: {
    winner: {
      type: String,
      default: "",
    },
    isBoardFull: {
      type: Boolean,
      default: false,
    },
  },
  methods: {},
};
</script>

<style scoped lang="scss">
.status {
  margin-top: 10px;
  text-align: center;
}
</style>
