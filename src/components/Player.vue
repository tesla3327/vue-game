<template>
  <sprite-view
    :position="position"
    :size="size"
    color="royalblue">
  </sprite-view>
</template>

<script>
import SpriteView from './Sprite.vue';

const BASE_SPEED = 10;

const handleKeyDown = function(e) {
  let velocity = {};

  switch (e.key) {
    case 'ArrowUp':
      velocity = { y: BASE_SPEED * -1 };
      break;

    case 'ArrowDown':
      velocity = { y: BASE_SPEED };
      break;

    case 'ArrowLeft':
      velocity = { x: BASE_SPEED * -1 };
      break;

    case 'ArrowRight':
      velocity = { x: BASE_SPEED };
      break;

    default:
      break;
  }

  this.$emit('velocity-changed', velocity);
};

const handleKeyUp = function(e) {
  let velocity = {};

  switch (e.key) {
    case 'ArrowUp':
    case 'ArrowDown':
      velocity = { y: 0 };
      break;

    case 'ArrowLeft':
    case 'ArrowRight':
      velocity = { x: 0 };
      break;

    default:
      break;
  }

  this.$emit('velocity-changed', velocity);
};

export default {
  name: 'PlayerView',
  components: {
    SpriteView,
  },
  props: {
    position: {
      type: Object,
      default: { x: 0, y: 0 },
    },
    size: {
      type: Number,
      default: 35,
    },
  },
  mounted() {
    // Bind our instance
    this.handleKeyDown = this.handleKeyDown.bind(this);
    this.handleKeyUp = this.handleKeyUp.bind(this);

    window.addEventListener('keydown', this.handleKeyDown);
    window.addEventListener('keyup', this.handleKeyUp);
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.handleKeyDown);
    window.removeEventListener('keyup', this.handleKeyUp);
  },
  methods: {
    handleKeyDown,
    handleKeyUp,
  },
};
</script>

<style lang="scss" scoped>
  .sprite {
    position: relative;
    border-radius: 100%;
  }
</style>
