<script>
export default {
  data() {
    return {
      floor: [],
      activeFloor: 1,
      top: 611,
      activeLift: false,
    };
  },
  methods: {
    move() {
      if (this.floor.length !== 0) {
        this.$refs.lift.style.top = `${this.top - (this.floor[0] - 1) * 151}px`;
        this.$refs.lift.style.transitionDuration = `${
          this.floor[0] - this.activeFloor
        }s`;
        setTimeout(() => {
          this.activeFloor = this.floor.shift();
          this.move();
        }, Math.abs(this.floor[0] - this.activeFloor) * 1000 + 3000);
      }
      if (this.floor.length === 0) {
        this.activeLift = false;
      }
    },
    add(floorNumber) {
      this.floor.push(floorNumber);
      if (!this.activeLift) {
        this.move();
        this.activeLift = true;
      }
    },
  },
};
</script>

<template>
  <div class="floor">
    <div class="lt"></div>
    <p>5</p>
    <button v-on:click="add(5)">add</button>
  </div>
  <div class="floor">
    <div class="lt"></div>
    <p>4</p>
    <button v-on:click="add(4)">add</button>
  </div>
  <div class="floor">
    <div class="lt"></div>
    <p>3</p>
    <button v-on:click="add(3)">add</button>
  </div>
  <div class="floor">
    <div class="lt"></div>
    <p>2</p>
    <button v-on:click="add(2)">add</button>
  </div>
  <div class="floor">
    <div class="lt"></div>
    <p>1</p>
    <button v-on:click="add(1)">add</button>
  </div>
  <div class="lift" ref="lift"></div>
  {{ floor }}
</template>

<style scoped>
.lt {
  height: 150px;
  width: 100px;
  border-bottom: 1px solid;
  border-right: 1px solid;
  border-left: 1px solid;
}
button {
  height: fit-content;
  margin-left: 2%;
  margin-top: 5%;
}
.floor {
  display: flex;
}
p {
  margin-left: 20px;
}
.lift {
  background-color: aqua;
  width: 99.5px;
  height: 150px;
  position: absolute;
  top: 610px;
  left: 9px;
  transition-property: all;
  transition-timing-function: ease-in-out;
  transition-delay: 0s;
}
</style>
