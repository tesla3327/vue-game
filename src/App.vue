<template>
  <div id="app">
    <h1>This is a game</h1>
    <map-view :width="mapWidth" :height="mapHeight">
      <items-view
        :number="5"
        :width="mapWidth"
        :height="mapHeight"
        :player-size="spriteSize"
        :item-size="15"
      ></items-view>
      <sprite-view
        :position="position"
        :size="spriteSize"
        color="royalblue">
      </sprite-view>
    </map-view>
  </div>
</template>

<script>
import MapView from './components/Map.vue';
import SpriteView from './components/Sprite.vue';
import ItemsView from './components/ItemsView.vue';

const BASE_SPEED = 3;

export default {
  name: 'app',
  components: {
    MapView,
    SpriteView,
    ItemsView,
  },
  mounted() {
    window.addEventListener('keydown', this.handleKeyDown.bind(this));
    window.addEventListener('keyup', this.handleKeyUp.bind(this));

    const updatePosition = () => {
      this.position.x += this.velocity.x;
      this.position.y += this.velocity.y;

      if (this.position.x < 0) {
        this.position.x = 0;
      } else if (this.position.x > this.mapWidth - this.spriteSize) {
        this.position.x = this.mapWidth - this.spriteSize;
      }

      if (this.position.y < 0) {
        this.position.y = 0;
      } else if (this.position.y > this.mapHeight - this.spriteSize) {
        this.position.y = this.mapHeight - this.spriteSize;
      }

      window.requestAnimationFrame(updatePosition);
    };
    window.requestAnimationFrame(updatePosition);
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.handleKeyDown.bind(this));
    window.removeEventListener('keyup', this.handleKeyUp.bind(this));
  },
  data() {
    return {
      position: { x: 300, y: 200 },
      velocity: { x: 0, y: 0 },
      mapWidth: 1000,
      mapHeight: 600,
      spriteSize: 35,
    };
  },
  methods: {
    handleKeyDown(e) {
      switch (e.key) {
        case 'ArrowUp':
          this.velocity.y = BASE_SPEED * -1;
          break;

        case 'ArrowDown':
          this.velocity.y = BASE_SPEED;
          break;

        case 'ArrowLeft':
          this.velocity.x = BASE_SPEED * -1;
          break;

        case 'ArrowRight':
          this.velocity.x = BASE_SPEED;
          break;

        default:
          break;
      }
    },

    handleKeyUp(e) {
      switch (e.key) {
        case 'ArrowUp':
        case 'ArrowDown':
          this.velocity.y = 0;
          break;

        case 'ArrowLeft':
        case 'ArrowRight':
          this.velocity.x = 0;
          break;

        default:
          break;
      }
    }
  }
}
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css?family=Love+Ya+Like+A+Sister');

  #app {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: 'Love Ya Like A Sister', cursive;
  }
</style>