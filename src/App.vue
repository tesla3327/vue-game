<template>
  <div id="app">
    <h1>This is a game</h1>
    <map-view :width="mapWidth" :height="mapHeight">
      <sprite-view
        :posx="posx"
        :posy="posy"
        :size="spriteSize">
      </sprite-view>
    </map-view>
  </div>
</template>

<script>
import MapView from './components/Map.vue';
import SpriteView from './components/Sprite.vue';

const SPEED = 3;

export default {
  name: 'app',
  components: {
    MapView,
    SpriteView,
  },
  mounted() {
    window.addEventListener('keydown', this.handleKeyDown.bind(this));
    window.addEventListener('keyup', this.handleKeyUp.bind(this));

    const updatePosition = () => {
      this.posx += this.velocity.x;
      this.posy += this.velocity.y;

      if (this.posx < 0) {
        this.posx = 0;
      } else if (this.posx > this.mapWidth - this.spriteSize) {
        this.posx = this.mapWidth - this.spriteSize;
      }

      if (this.posy < 0) {
        this.posy = 0;
      } else if (this.posy > this.mapHeight - this.spriteSize) {
        this.posy = this.mapHeight - this.spriteSize;
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
      posx: 300,
      posy: 200,
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
          this.velocity.y = SPEED * -1;
          break;

        case 'ArrowDown':
          this.velocity.y = SPEED;
          break;

        case 'ArrowLeft':
          this.velocity.x = SPEED * -1;
          break;

        case 'ArrowRight':
          this.velocity.x = SPEED;
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