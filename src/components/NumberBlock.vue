<template>
<transition name="appear">
  <div :class="className" :style="{left: mSlot.x + 'px', top: mSlot.y + 'px'}">{{ num }}</div>
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

.t-one      {background-color: orange;}
.t-two      {background-color: yellowgreen;}
.t-three    {background-color: red;}
.t-four     {background-color: blueviolet;}
.t-five     {background-color: yellow;}
.t-six      {background-color: orange;}
.t-seven    {background-color: orange;}
.t-eight    {background-color: orange;}
.t-nine     {background-color: orange;}
.t-ten      {background-color: orange;}
.t-eleven   {background-color: orange;}
.t-twelve   {background-color: orange;}
.t-thirteen {background-color: orange;}
.t-fourteen {background-color: orange;}
</style>
