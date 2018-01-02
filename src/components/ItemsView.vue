<template>
  <div class="items-view">
    <sprite-view
      v-for="sprite in items"
      :key="sprite"
      :position="sprite"
      :size="itemSize"
      color="white">
    </sprite-view>
  </div>
</template>

<script>
import SpriteView from './Sprite.vue';

const getRandom = (min, max) => {
  return Math.floor(Math.random() * (max - min)) + min;
};

export default {
  name: 'ItemsView',
  components: {
    SpriteView,
  },
  mounted() {
    // Randomly generate a list of items
    this.items = new Array(this.number)
      .fill(null)
      .map(() => ({
        x: getRandom(this.playerSize, this.width - this.playerSize),
        y: getRandom(this.playerSize, this.height - this.playerSize),
      }));
  },
  data() {
    return {
      items: [],
    };
  },
  props: {
    number: Number,
    width: Number,
    height: Number,
    playerSize: Number,
    itemSize: Number,
  },
}
</script>

<style lang="scss" scoped>
  .items-view {
    height: 0;
  }
</style>
