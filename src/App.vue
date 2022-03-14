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
      shouldAddBlock: Boolean,
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
      this.shouldAddBlock = false
      switch (key) {
        case 'ArrowUp':
          // console.log(key)
          this.moveToUp()
          this.mergeToUp()
          this.moveToUp()
          break
        case 'ArrowDown':
          // console.log(key)
          this.moveToBottom()
          this.mergeToBottom()
          this.moveToBottom()
          break
        case 'ArrowLeft':
          // console.log(key)
          this.moveToLeft()
          this.mergeToLeft()
          this.moveToLeft()
          break
        case 'ArrowRight':
          // console.log(key)
          this.moveToRight()
          this.mergeToRight()
          this.moveToRight()
          break
        default:
          return
      }
      if (this.shouldAddBlock) {
        this.addNumberBlock()
      }
    },
    setSlotEmpty(slotId) {
      this.slotBlocks[slotId] = {
        blockId: '',
        hasBlock: false,
        num: 0
      }
      this.emptySlots.push(slotId)
    },
    setSlotFull(slotFrom, slotTo) {
      this.emptySlots = this.emptySlots.filter(id => id !== slotTo)
      this.slotBlocks[slotTo] = this.slotBlocks[slotFrom]
    },
    moveBlock(from, to) {
      this.setSlotFull(from, to)
      this.setSlotEmpty(from)
      this.blocks.forEach(block => {
        if (block.slotId === from) {
          block.slotId = to
        }
      })
      this.shouldAddBlock = true
    },
    moveToUp() {
      // console.log('MOVE TO UP')
      // Start from 0 to ensure bolcks at top moved first
      for (let i = 0; i < 16; i++) {
        if (this.slotBlocks[i].hasBlock) {
          let to = i % 4
          while (to < i && this.slotBlocks[to].hasBlock) {
            to += 4
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    moveToBottom() {
      // console.log('MOVE TO BOTTOM')
      // Start from 15 to ensure bolcks at bottom moved first
      for (let i = 15; i >= 0; i--) {
        if (this.slotBlocks[i].hasBlock) {
          let to = 12 + i % 4
          while (to > i && this.slotBlocks[to].hasBlock) {
            to -= 4
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    moveToLeft() {
      // console.log('MOVE TO LEFT')
      // Start from 0 to ensure bolcks at left moved first
      for (let i = 0; i < 16; i++) {
        if (this.slotBlocks[i].hasBlock) {
          let to = Math.floor(i / 4) * 4
          while (to < i && this.slotBlocks[to].hasBlock) {
            to += 1
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    moveToRight() {
      // console.log('MOVE TO RIGHT')
      // Start from 15 to ensure bolcks at right moved first
      for (let i = 15; i >= 0; i--) {
        if (this.slotBlocks[i].hasBlock) {
          let to = Math.floor(i / 4) * 4 + 3
          while (to > i && this.slotBlocks[to].hasBlock) {
            to -= 1
          }
          if (to !== i) {
            this.moveBlock(i, to)
          }
        }
      }
    },
    mergeToUp() {
      for (let i = 0; i < (16 - 4); i++) {
        if (this.slotBlocks[i].hasBlock && this.slotBlocks[i + 4].hasBlock &&
        this.slotBlocks[i].num === this.slotBlocks[i + 4].num) {
          this.removeBlock(i + 4)
          this.slotBlocks[i].num = this.slotBlocks[i].num * 2
        }
      }
    },
    mergeToBottom() {
      for (let i = 15; i >= 4; i--) {
        if (this.slotBlocks[i].hasBlock && this.slotBlocks[i - 4].hasBlock &&
        this.slotBlocks[i].num === this.slotBlocks[i - 4].num) {
          this.removeBlock(i - 4)
          this.slotBlocks[i].num = this.slotBlocks[i].num * 2
        }
      }
    },
    mergeToLeft() {
      for (let i = 0; i < 16; i++) {
        if (i % 4 === 3) continue
        if (this.slotBlocks[i].hasBlock && this.slotBlocks[i + 1].hasBlock &&
        this.slotBlocks[i].num === this.slotBlocks[i + 1].num) {
          this.removeBlock(i + 1)
          this.slotBlocks[i].num = this.slotBlocks[i].num * 2
        }
      }
    },
    mergeToRight() {
      for (let i = 15; i >= 0; i--) {
        if (i % 4 === 0) continue
        if (this.slotBlocks[i].hasBlock && this.slotBlocks[i - 1].hasBlock &&
        this.slotBlocks[i].num === this.slotBlocks[i - 1].num) {
          this.removeBlock(i - 1)
          this.slotBlocks[i].num = this.slotBlocks[i].num * 2
        }
      }
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
    removeBlock(slotId) {
      this.blocks = this.blocks.filter(block => block.slotId !== slotId)
      this.setSlotEmpty(slotId)
      // Should add new block if there has blocks merged
      this.shouldAddBlock = true
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
