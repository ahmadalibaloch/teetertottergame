<template>
  <div class="teeter" :style="style"></div>
</template>

<script>
export default {
  name: "Teeter",
  props: {
    leftWeight: Number,
    rightWeight: Number
  },
  computed: {
    getRotation() {
      const rotation = this.leftWeight - this.rightWeight;
      this.emitRotation(rotation);
      return rotation;
    },
    style() {
      return {
        transform: `rotate(${-this.getRotation}deg)`
      };
    }
  },
  methods: {
    emitRotation(rotation) {
      this.$emit("rotated", rotation);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.teeter {
  position: absolute;
  background-color: brown;
}
</style>
