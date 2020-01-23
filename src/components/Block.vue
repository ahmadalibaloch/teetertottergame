<template>
  <div class="block" :style="style"></div>
</template>

<script>
import { TEETER_TOP } from "../App";
export default {
  name: "Block",
  props: {
    type: String,
    size: String,
    pos: Array,
    color: String,
    weight: Number
  },
  methods: {
    move() {
      this.gravitySpeed += this.gravity;
      this.position = [
        this.position[0],
        this.position[1] + 1 + this.gravitySpeed
      ];
      if (this.position[1] > TEETER_TOP) {
        this.$emit("hitTeeter", this.weight);
        clearInterval(this.moveInterval);
      }
    }
  },
  data() {
    return {
      position: [...this.$props.pos],
      rotation: `rotate(${Math.random() * 180}deg)`,
      gravity: 1.05,
      gravitySpeed: 0
    };
  },
  created() {
    this.position = [...this.pos];
    this.moveInterval = setInterval(() => {
      this.move();
      if (this.position[1] > 1000) {
        clearInterval(this.moveInterval);
      }
    }, 100);
  },
  computed: {
    style() {
      let width = 50;
      let height = 50;
      let zIndex = 5;
      switch (this.size) {
        case "small":
          width = 30;
          height = 30;
          zIndex = 4;
          break;
        case "medium":
          width = 40;
          height = 50;
          zIndex = 3;
          break;
        case "large":
          width = 50;
          height = 50;
          zIndex = 2;
          break;
        default:
          width = 60;
          height = 60;
          zIndex = 1;
      }
      return {
        left: `${this.position[0]}px`,
        top: `${this.position[1]}px`,
        width: `${width}px`,
        height: `${height}px`,
        zIndex,
        transform: this.rotation,
        backgroundColor: this.color
      };
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.block {
  position: absolute;
  border: 1px solid rgb(75, 70, 70);
}
</style>
