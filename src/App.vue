<template>
  <div id="app">
    <h1>This is not a game</h1>
    <h2>{{ points }}</h2>
    <map-view :width="mapWidth" :height="mapHeight">
      <items-view
        :number="10"
        :item-size="15"
        :bounding-box="itemsBoundingBox"
        :player-bounding-box="playerBoundingBox"
        @earned-points="accumulatePoints"
      ></items-view>
      <player-view
        :position="playerPos"
        :size="spriteSize"
        @velocity-changed="updateVelocity"
        color="royalblue">
      </player-view>
    </map-view>
  </div>
</template>

<script>
import MapView from './components/Map.vue';
import PlayerView from './components/Player.vue';
import ItemsView from './components/ItemsView.vue';

// Constrain a position so it stays within a bounding box
const constrainPosToBox = (topLeft, bottomRight, pos) => {
  let x = Math.max(topLeft.x, pos.x);
  x = Math.min(bottomRight.x, x);

  let y = Math.max(topLeft.y, pos.y);
  y = Math.min(bottomRight.y, y);

  return { x, y };
};

export default {
  name: 'app',
  components: {
    MapView,
    PlayerView,
    ItemsView,
  },
  mounted() {
    // Start the loop to update player position
    window.requestAnimationFrame(this.updatePlayerPosition);
  },
  data() {
    return {
      playerPos: { x: 300, y: 200 },
      playerVelocity: { x: 0, y: 0 },
      mapWidth: 1000,
      mapHeight: 600,
      spriteSize: 35,
      points: 0,
    };
  },
  computed: {
    // Create a box just smaller than the map so we can reach all items
    itemsBoundingBox() {
      const buffer = this.spriteSize
      return {
        topLeft: {
          x: buffer,
          y: buffer,
        },
        bottomRight: {
          x: this.mapWidth - buffer,
          y: this.mapHeight - buffer,
        },
      };
    },
    playerBoundingBox() {
      return {
        topLeft: this.playerPos,
        bottomRight: {
          x: this.playerPos.x + this.spriteSize,
          y: this.playerPos.y + this.spriteSize,
        }
      }
    }
  },
  methods: {
    accumulatePoints(points) {
      this.points += points;
    },
    updateVelocity(newVelocity) {
      // Merge in changes
      this.playerVelocity = Object.assign(this.playerVelocity, newVelocity);
    },
    updatePlayerPosition() {
      let newPos = {
        x: this.playerPos.x + this.playerVelocity.x,
        y: this.playerPos.y + this.playerVelocity.y,
      };
      
      newPos = constrainPosToBox(
        { x: 0, y: 0 },
        {
          x: this.mapWidth - this.spriteSize,
          y: this.mapHeight - this.spriteSize
        },
        newPos
      );

      if (newPos.x !== this.playerPos.x ||
          newPos.y !== this.playerPos.y) {
        this.playerPos = newPos;
      }

      window.requestAnimationFrame(this.updatePlayerPosition);
    },
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