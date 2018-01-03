<template>
  <div class="sprite"
       :style="styles">
  </div>
</template>

<script>
export default {
  name: 'SpriteView',
  props: {
    position: Object,
    size: Number,
    color: String,
  },
  computed: {
    styles() {
      return {
        top: `${this.position.y}px`,
        left: `${this.position.x}px`,
        width: `${this.size}px`,
        height: `${this.size}px`,
        background: this.color,
      }
    }
  },
}
</script>

<style lang="scss">
  .sprite {
    position: absolute;
    border-radius: 100%;
  }
</style>
