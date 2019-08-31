<template>
  <div @click="onClose" class="close-icon"><span>×</span></div>
</template>

<script>
  export default {
    name: "CloseIcon",
    methods: {
      onClose () {
        this.$emit('onClose')
      }
    }
  }
</script>

<style scoped lang="scss">
.close-icon {
  position: absolute;
  z-index: 1;
  height: 16px;
  width: 16px;

  margin: 3.5px 4px;

  border-radius: 100px;
  background-color: rgba(105, 105, 105, 0.4);

  &:hover {
    cursor: pointer;
    background-color: rgb(232, 248, 248);

    span {
      color: #8098cf
    }
  }
  span {
    display: block;
    color: rgb(250, 250, 250);
    margin-top: -4.5px;
  }
}
</style>
