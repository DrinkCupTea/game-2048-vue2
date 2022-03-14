<template>
  <div id="app">
    <header id="header">
      <p>Start Game</p>
      <p>Score 100</p>
    </header>
    <div id="container">
      <!-- <NumberBlock v-for="block in blocks" :key="block.id" :num="block.num" :mSlot="SLOTS_INFO[block.slotId]"></NumberBlock> -->
      <NumberBlock v-for="block in blocks" :key="block.id" :num="slotBlocks[block.slotId].num" :mSlot="SLOTS_INFO[block.slotId]"></NumberBlock>
    </div>
  </div>
</template>

<script>
import { v4 as uuidv4 } from 'uuid'

import NumberBlock from '@/components/NumberBlock.vue'

export default {
  name: 'App',
  components: {
    NumberBlock
  },
  data() {
    return {
      show: Boolean,
      SLOTS_INFO: [],
      blocks: [],
      emptySlots: [],
      slotBlocks: []
    }
  },
  methods: {
    initData() {
      this.show = true
      for (let i = 0; i < 4; i++) {
        for (let j = 0; j < 4; j++) {
          this.SLOTS_INFO.push({
            x: j * 100 + 10,
            y: i * 100 + 10
          })
        }
      }
      for (let i = 0; i < 16; i++) {
        this.emptySlots.push(i)
        this.slotBlocks.push({
          blockId: '',
          hasBlock: false,
          num: 0
        })
      }
    },
    move(e) {
      const key = e.key
      let moved = false
      switch (key) {
        case 'ArrowUp':
          // console.log(key)
          moved = this.moveToUp()
          this.mergeToUp()
          break
        case 'ArrowDown':
          // console.log(key)
          moved = this.moveToBottom()
          this.mergeToBottom()
          break
        case 'ArrowLeft':
          // console.log(key)
          moved = this.moveToLeft()
          this.mergeToLeft()
          break
        case 'ArrowRight':
          // console.log(key)
          moved = this.moveToRight()
          this.mergeToRight()
          break
        default:
          return
      }
      if (moved) {
        this.addNumberBlock()
      }
    },
    setSlotEmpty(from) {
      this.slotBlocks[from] = {
        blockId: '',
        hasBlock: false,
        num: 0
      }
      this.emptySlots.push(from)
    },
    setSlotFull(from, to) {
      this.emptySlots = this.emptySlots.filter(id => id !== to)
      this.slotBlocks[to] = this.slotBlocks[from]
    },
    moveBlock(from, to) {
      this.setSlotFull(from, to)
      this.setSlotEmpty(from)
      this.blocks.forEach(block => {
        if (block.slotId === from) {
          block.slotId = to
        }
      })
    },
    moveToUp() {
      // console.log('MOVE TO UP')
      // Start from 0 to ensure bolcks at top moved first
      let moved = false
      for (let i = 0; i < 16; i++) {
        if (this.slotBlocks[i].hasBlock) {
          let to = i % 4
          while (to < i && this.slotBlocks[to].hasBlock) {
            to += 4
          }
          if (to !== i) {
            this.moveBlock(i, to)
            moved = true
          }
        }
      }
      return moved
    },
    moveToBottom() {
      // console.log('MOVE TO BOTTOM')
      // Start from 15 to ensure bolcks at bottom moved first
      let moved = false
      for (let i = 15; i >= 0; i--) {
        if (this.slotBlocks[i].hasBlock) {
          let to = 12 + i % 4
          while (to > i && this.slotBlocks[to].hasBlock) {
            to -= 4
          }
          if (to !== i) {
            this.moveBlock(i, to)
            moved = true
          }
        }
      }
      return moved
    },
    moveToLeft() {
      // console.log('MOVE TO LEFT')
      // Start from 0 to ensure bolcks at left moved first
      let moved = false
      for (let i = 0; i < 16; i++) {
        if (this.slotBlocks[i].hasBlock) {
          let to = Math.floor(i / 4) * 4
          while (to < i && this.slotBlocks[to].hasBlock) {
            to += 1
          }
          if (to !== i) {
            this.moveBlock(i, to)
            moved = true
          }
        }
      }
      return moved
    },
    moveToRight() {
      // console.log('MOVE TO RIGHT')
      // Start from 15 to ensure bolcks at right moved first
      let moved = false
      for (let i = 15; i >= 0; i--) {
        if (this.slotBlocks[i].hasBlock) {
          let to = Math.floor(i / 4) * 4 + 3
          while (to > i && this.slotBlocks[to].hasBlock) {
            to -= 1
          }
          if (to !== i) {
            this.moveBlock(i, to)
            moved = true
          }
        }
      }
      return moved
    },
    mergeToUp() {
    },
    mergeToBottom() {
    },
    mergeToLeft() {
    },
    mergeToRight() {
    },
    addNumberBlock() {
      if (this.emptySlots.length === 0) {
        return
      }
      const slotId = this.emptySlots[Math.floor(Math.random() * this.emptySlots.length)]
      this.emptySlots = this.emptySlots.filter(id => id !== slotId)
      // this.slotBlocks[slotId].hasBlock = true
      const num = Math.random() >= 0.7 ? 4 : 2
      const id = uuidv4()
      this.blocks.push({
        id: id,
        slotId: slotId
      })
      this.slotBlocks[slotId] = {
        blockId: id,
        hasBlock: true,
        num: num
      }
    },
    removeBlock(id) {
      this.blocks = this.blocks.filter(block => block.slotId !== id)
      this.emptySlots.push(id)
    }
  },
  created() {
    window.addEventListener('keyup', this.move)
    this.initData()
    this.addNumberBlock()
  }
}
</script>

<style lang="scss">

body {
  margin: 0;
}

#app {
  background-color: cadetblue;
  width: 100%;
  height: 100%;
  position: fixed;
}

#header {
  p {
    color: white;
    font-size: 35px;
    text-align: center;
  }
}

#container {
  width: 400px;
  height: 400px;
  background-color: rgba($color: #EEEEEE, $alpha: 0.7);
  border-radius: 10px;
  margin-left: auto;
  margin-right: auto;
  position: relative;
}

button {
  text-align: center;
}

</style>
