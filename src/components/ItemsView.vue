<template>
  <div class="items-view">
    <sprite-view
      v-for="sprite in items"
      :key="`${sprite.x}-${sprite.y}`"
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

const boundingBoxCollision = a => b => {
  if (a.topLeft.x > b.bottomRight.x ||  // B is left of A
      a.topLeft.y > b.bottomRight.y ||  // B is above A
      a.bottomRight.x < b.topLeft.x ||  // B is right of A
      a.bottomRight.y < b.topLeft.y) {  // B is below A
    return false;
  } else {
    return true;
  }
};

export default {
  name: 'ItemsView',
  components: {
    SpriteView,
  },
  props: {
    number: Number,
    itemSize: Number,
    boundingBox: Object,
    playerBoundingBox: Object,
  },
  mounted() {
    // Randomly generate a list of items
    this.items = new Array(this.number)
      .fill(null)
      .map(this.generateItem.bind(this));

    // Add more items over time
    setInterval(() => {
      this.items.splice(this.items.length, 0, this.generateItem());
    }, 500);
  },
  data() {
    return {
      items: [],
    };
  },
  watch: {
    playerBoundingBox(newBox, oldBox) {
      const checkForCollision = boundingBoxCollision(newBox);
      const collisionItems = this.items.filter(item => {
        return checkForCollision({
          topLeft: item,
          bottomRight: {
            x: item.x + this.itemSize,
            y: item.y + this.itemSize,
          },
        });
      });

      // Remove all items that have been hit
      collisionItems.forEach(item => {
        const index = this.items.indexOf(item);
        this.items.splice(index, 1);
        this.$emit('earned-points', 100);
      });
    },
  },
  methods: {
    generateItem() {
      return {
        x: getRandom(this.boundingBox.topLeft.x, this.boundingBox.bottomRight.x),
        y: getRandom(this.boundingBox.topLeft.y, this.boundingBox.bottomRight.y),
      };
    }
  },
}
</script>

<style lang="scss" scoped>
  .items-view {
    position: relative;
    height: 0;
  }
</style>
