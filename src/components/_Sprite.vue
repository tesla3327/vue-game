<template>
  <div class="sprite"
       :style="styles">
  </div>
</template>

<script>
export default {
  name: 'ItemsView',
  mounted() {
    // Randomly generate a list of items
    this.items = Array.new(this.number).fill(10);
    console.log(this.items);
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
  },
}
</script>

<style lang="scss" scoped>
  .sprite {
    position: relative;
    border-radius: 100%;
  }
</style>
