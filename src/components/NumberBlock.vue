<template>
  <transition
    name="appear"
  >
    <div
      :class="className"
      :style="{left: mSlot.x + 'px', top: mSlot.y + 'px'}"
      v-if="show"
    >{{ num }}</div>
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
      default: true
    },
    mSlot: {
      typeof: {},
      default: {
        id: 0,
        x: 10,
        y: 50
      }
    }
  },
  data() {
    return {
      className: String
    }
  },
  watch: {
    num(newNum) {
      // console.log(newNum)
      this.updateClass(newNum)
    }
  },
  methods: {
    updateClass(num) {
      const classList = ['t-one', 't-two', 't-three', 't-four', 't-five', 't-six', 't-seven', 't-eight', 't-nine', 't-ten', 't-eleven', 't-twelve', 't-thirteen', 't-fourteen']
      let t = 0
      while (num > 2) {
        num /= 2
        t += 1
      }
      this.className = classList[t]
    }
  },
  created() {
    this.updateClass(this.num)
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
  transition: 100ms;
}

.appear-enter-active {
  animation: appear 150ms ease-in-out;
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

.t-one      {background-color: rgb(163, 135, 79);}
.t-two      {background-color: rgb(175, 139, 70);}
.t-three    {background-color: rgb(248, 175, 39);}
.t-four     {background-color: rgb(255, 123, 0);}
.t-five     {background-color: rgb(192, 18, 18);}
.t-six      {background-color: red;}
.t-seven    {background-color: rgb(209, 209, 56);font-size: 40px;}
.t-eight    {background-color: yellowgreen;font-size: 40px;}
.t-nine     {background-color: blueviolet;font-size: 40px;}
.t-ten      {background-color: paleturquoise;font-size: 33px;}
.t-eleven   {background-color: palegreen;font-size: 33px;}
.t-twelve   {background-color: cornflowerblue;font-size: 33px;}
.t-thirteen {background-color: greenyellow;font-size: 33px;}
.t-fourteen {background-color: gold;font-size: 33px;}
</style>
