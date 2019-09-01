<template>
  <div v-bind:style="styles" class="loader"></div>
</template>

<script>
  export default {
    props: {
      size: {
        default: '60px'
      }
    },

    data () {
      return {
        styles: {
          width: this.size,
          height: this.size
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  .loader {
    position: relative;
    &:before {
      content: '';
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 20%;
      background: rgba(128, 128, 128, 0.3);
      border-radius: 50%;
      animation: shadow-animate 1.8s linear infinite;
    }
    &:after {
      content: '';
      width: 100%;
      height: 100%;
      background: #adc3e3;
      animation: ball-animation 1.8s linear infinite;
      position: absolute;
      bottom:40%;
      transform: rotate(45deg);
      border-radius: 50%;
    }
  }

  @keyframes shadow-animate {
    0%, 100% {
      transform: translateX(0) scale(1.4, 1);
    }

    25%, 75% {
      transform: translateX(200%) scale(0.8, 1);
    }

    50% {
      transform: translateX(400%) scale(1.4, 1);
    }
  }

  @keyframes ball-animation {
    0%, 100% {
      transform: translateX(0) translateY(60%) scale(1.3, 0.5);
    }

    25%, 75% {
      transform: translateX(200%) translateY(0) scale(0.8, 1);
    }

    50% {
      transform: translateX(400%) translateY(60%) scale(1.3, 0.5);
    }
  }
</style>
