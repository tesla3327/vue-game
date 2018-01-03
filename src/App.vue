<template>
  <div id="app">
    <h1>This is not a game</h1>
    <map-view :width="mapWidth" :height="mapHeight">
      <items-view
        :number="5"
        :width="mapWidth"
        :height="mapHeight"
        :player-size="spriteSize"
        :item-size="15"
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
    };
  },
  methods: {
    updateVelocity(newVelocity) {
      // Merge in changes
      this.playerVelocity = Object.assign(this.playerVelocity, newVelocity);
    },
    updatePlayerPosition() {
      this.playerPos.x += this.playerVelocity.x;
      this.playerPos.y += this.playerVelocity.y;
      
      this.playerPos = constrainPosToBox(
        { x: 0, y: 0 },
        {
          x: this.mapWidth - this.spriteSize,
          y: this.mapHeight - this.spriteSize
        },
        this.playerPos
      );

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