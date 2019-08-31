<template>
  <div
      class="close-icon"
      @click="$emit('click')"
      @mouseover="changeToHoverText"
      @mouseleave="changeToDefaultButtonText"
      >
    <span :class="{ 'animated-text': isHoverTextEnable }">{{ buttonText }}</span>
  </div>
</template>

<script>
  export default {
    name: "CloseIcon",
    props: {
      isHoverTextEnable: {
        type: Boolean,
        default: true
      },
      hoverText: {
        type: String,
        default: 'close'
      }
    },
    methods: {
      changeToHoverText () {
        if (this.isHoverTextEnable) {
          this.buttonText = this.hoverText
        }
      },

      changeToDefaultButtonText () {
        if (this.isHoverTextEnable) {
          this.buttonText = '×'
        }
      },

    },
    data () {
      return {
        buttonText: '×'
      }
    }
  }
</script>

<style scoped lang="scss">
.close-icon {
  position: absolute;
  z-index: 1;
  height: 16px;
  min-width: 16px;
  padding-right: 3px;
  padding-left: 3px;
  margin: 3.5px 4px;

  border-radius: 100px;
  background-color: rgba(105, 105, 105, 0.4);

  &:hover {
    cursor: pointer;
    background-color: rgb(232, 248, 248);

    span {
      color: #6a7fae;

      &.animated-text {
        font-size: 12px;
        padding-top: 4px;
        padding-left: 10px;
        padding-right: 10px;
      }
    }
  }
  span {
    display: block;
    color: rgb(250, 250, 250);
    margin-top: -4.5px;
  }
}
</style>
