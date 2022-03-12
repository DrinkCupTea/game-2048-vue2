<template>
<transition name="appear">
  <div :class="className" v-if="show" :style="{left: x + 'px', top: y + 'px'}">{{ num }}</div>
</transition>
</template>

<script>
export default {
  name: 'NumberBlock',
  props: {
    num: {
      typeof: Number,
      default: 2
    },
    show: {
      typeof: Boolean,
      default: true,
      x: Number,
      y: Number
    },
    ix: Number,
    iy: Number
  },
  data() {
    return {
      className: String
    }
  },
  created() {
    switch (this.num) {
      case 2:
        this.className = 'two'
        break
      case 4:
        this.className = 'four'
        break
      default:
        this.className = 'two'
    }
    this.x = this.ix
    this.y = this.iy
    // console.log(this.x, this.y)
  }
}
</script>

<style lang="scss" scoped>
div {
  width: 80px;
  height: 80px;
  border-radius: 10px;
  color: white;
  text-align: center;
  font-size: 50px;
  font-weight: bold;
  font-family: Arial, Helvetica, sans-serif;
  line-height: 80px;
  position: absolute;
}

.two {
  background-color: orange;
}

.four {
  background-color: yellowgreen;
}

.appear-enter-active {
  animation: appear 100ms ease-in-out;
}

@keyframes appear {
  0% {
    opacity: 0;
    transform: scale(0);
  }
  50% {
    opacity: 0;
    transform: scale(0.5);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}
</style>
