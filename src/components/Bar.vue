<template>
  <div class="bar"></div>
</template>

<script>
export default {
  name: "Bar",
  props: {
    height: Number,
    width: Number,
    pos: Array
  },
  computed: {
    style() {
      return {
        width: this.width + "px",
        height: this.height + "px",
        left: this.pos[0] + "px",
        top: this.pos[1] + "px"
      };
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.bar {
  width: 0;
  height: 0;
  border-left: 35px solid transparent;
  border-right: 35px solid transparent;

  border-bottom: 50px solid black;
  position: absolute;
}
</style>
