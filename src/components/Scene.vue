<template>
  <div class="scene" :style="style">
    <span class="leftMsg">Left Weight: {{leftWeight}}</span>
    <span class="rightMsg">Right Weight: {{rightWeight}}</span>
    <TeeterTotter
      :style="teeterStyle"
      :leftWeight="leftWeight"
      :rightWeight="rightWeight"
      v-on:rotated="teeterRotated"
    ></TeeterTotter>
    <Bar :style="barStyle"></Bar>
    <Block
      v-for="block in leftBlocks"
      :pos="block.pos"
      :size="block.size"
      v-bind:key="block.id"
      :color="block.color"
      :weight="block.weight"
      v-on:hitTeeter="hitTeeter"
    ></Block>
    <Block
      v-for="block in rightBlocks"
      :pos="block.pos"
      :size="block.size"
      v-bind:key="block.id"
      :color="block.color"
    ></Block>
  </div>
</template>

<script>
import TeeterTotter from "./Teeter";
import Bar from "./Bar";
import Block from "./Block";

const calcPosLoc = (height, width, pos) => {
  return {
    left: pos[0] + "px",
    top: pos[1] + "px",
    width: width + "px",
    height: height + "px"
  };
};

export default {
  name: "Scene",
  components: {
    TeeterTotter,
    Bar,
    Block
  },
  props: {
    height: Number,
    width: Number,
    pos: Array,
    bar: Object,
    teeter: Object,
    rightBlocks: Array,
    leftBlocks: Array
  },
  data() {
    return {
      teeterRotated: rotation => {
        // change the boxes location and also calcuate next hit point
        if (Math.abs(rotation) > 30) {
          this.$emit("endAnimation");
        }
      },
      hitTeeter: weight => {
        this.leftWeight += weight;
        if (Math.abs(this.leftWeight - this.rightWeight) > 20) {
          this.$emit("endAnimation");
        }
      },
      leftWeight: this.$props.leftBlocks.reduce(
        (weight, b) => weight + b.weight,
        0
      ),
      rightWeight: this.$props.rightBlocks.reduce(
        (weight, b) => weight + b.weight,
        0
      )
    };
  },
  computed: {
    teeterStyle() {
      const width = this.teeter.length * 50;
      const height = this.teeter.radius * 2;
      const pos = [
        this.teeter.pos[0] - width / 2,
        this.teeter.pos[1] - height / 2
      ];
      return calcPosLoc(height, width, pos);
    },
    barStyle() {
      return calcPosLoc(this.bar.height, this.bar.width, this.bar.pos);
    },
    style() {
      return calcPosLoc(this.height, this.width, this.pos);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.scene {
  background-color: rgb(228, 224, 224);
  border: 2px solid rgb(88, 79, 79);
  position: absolute;
}
.leftMsg {
  position: absolute;
  left: 10px;
}
.rightMsg {
  position: absolute;
  left: 650px;
}
</style>
