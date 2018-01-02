<template>
  <div class="map" v-bind:style="styles">
    <slot />
  </div>
</template>

<script>
export default {
  name: 'MapView',
  props: {
    width: {
      type: Number,
      default: 800,
    },
    height: {
      type: Number,
      default: 500,
    }
  },
  computed: {
    styles () {
      return {
        width: `${this.width}px`,
        height: `${this.height}px`,
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .map {
    background: rgb(200,220,200);
  }
</style>
