<template>
  <div id="app">
    <Scene
      :pos="pos"
      :height="height"
      :width="width"
      :bar="bar"
      :teeter="teeter"
      :leftBlocks="leftBlocks"
      :rightBlocks="rightBlocks"
      v-on:endAnimation="endAnimation"
    />
  </div>
</template>

<script>
import Scene from "./components/Scene.vue";

// the scene or context
const SCENE_HEIGHT = 450;
const SCENE_WIDTH = 800;

// The teetar on bar, telling weights
export const TEETER_TOP = 400;
const TEETER_LEFT = 400;
const TEETER_LENGTH = 10;
const TEETER_RADIUS = 2;

/**
 * Create random block on left or right side
 */
const createBlock = (id, side = "left") => {
  const randomIndex = Math.floor(Math.random() * 3); // 1, 2, 3 , small, medium, large, x-l
  const size = ["small", "medium", "large"][randomIndex];
  const color = ["red", "green", "blue", "yello"][randomIndex];
  const weight = 1 + Math.floor(Math.random() * 10); // weight between 1-10
  const pos = [150 + Math.random() * 200, 0]; // within seen left part, withtin height
  if (side == "right") {
    pos[0] += 300;
    pos[1] = 400;
  }
  return { size, pos, id, color, weight };
};
let animationInterval;
export default {
  name: "app",
  components: {
    Scene
  },
  data() {
    return {
      pos: [300, 50],
      height: SCENE_HEIGHT,
      width: SCENE_WIDTH,
      bar: { pos: [TEETER_LEFT - 50, TEETER_TOP - 50], height: 50, width: 20 },
      teeter: {
        pos: [TEETER_LEFT, TEETER_TOP],
        length: TEETER_LENGTH,
        radius: TEETER_RADIUS
      },
      rightBlocks: Array.from({ length: 3 }).map((_, i) =>
        createBlock(i, "right")
      ),
      leftBlocks: [],
      endAnimation() {
        clearInterval(animationInterval);
      }
    };
  },
  created() {
    animationInterval = setInterval(() => {
      this.leftBlocks.push(createBlock());
    }, 3000);
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
