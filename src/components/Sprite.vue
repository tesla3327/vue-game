<template>
  <div class="sprite"
       :style="styles">
  </div>
</template>

<script>
export default {
  name: 'SpriteView',
  props: {
    posy: Number,
    posx: Number,
    size: Number,
  },
  computed: {
    styles() {
      return {
        top: `${this.posy}px`,
        left: `${this.posx}px`,
        width: `${this.size}px`,
        height: `${this.size}px`,
      }
    }
  },
}
</script>

<style lang="scss" scoped>
  .sprite {
    position: relative;
    border: 1px solid black;
    border-radius: 100%;
    background: royalblue;
  }
</style>
