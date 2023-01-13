<script>
export default {
  data() {
    return {
      floor: [],
      activeFloor: 1,
      top: 608,
      activeLift: false,
      isWaiting: false,
    };
  },
  methods: {
    onLeaveCanceled() {},
    move() {
      if (this.floor.length !== 0) {
        this.$refs.lift.style.top = `${this.top - (this.floor[0] - 1) * 150}px`;
        this.$refs.lift.style.transitionDuration = `${
          this.floor[0] - this.activeFloor
        }s`;
        let moveDuring = Math.abs(this.floor[0] - this.activeFloor) * 1000;
        setTimeout(() => {
          this.activeFloor = this.floor.shift();
          this.isWaiting = true;
        }, moveDuring);
        setTimeout(() => {
          this.move();
          this.isWaiting = false;
        }, moveDuring + 3000);
      }
      if (this.floor.length === 0) {
        this.activeLift = false;
      }
    },
    add(floorNumber) {
      if (
        !this.floor.includes(floorNumber) &&
        this.activeFloor !== floorNumber
      ) {
        this.floor.push(floorNumber);
      }
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
    <button
      v-on:click="add(5)"
      v-bind:class="{ activeButton: floor.includes(5) }"
    >
      &otimes;
    </button>
  </div>
  <div class="floor">
    <div class="lt"></div>
    <p>4</p>
    <button
      v-on:click="add(4)"
      v-bind:class="{ activeButton: floor.includes(4) }"
    >
      &otimes;
    </button>
  </div>
  <div class="floor">
    <div class="lt"></div>
    <p>3</p>
    <button
      v-on:click="add(3)"
      v-bind:class="{ activeButton: floor.includes(3) }"
    >
      &otimes;
    </button>
  </div>
  <div class="floor">
    <div class="lt"></div>
    <p>2</p>
    <button
      v-on:click="add(2)"
      v-bind:class="{ activeButton: floor.includes(2) }"
    >
      &otimes;
    </button>
  </div>
  <div class="floor">
    <div class="lt"></div>
    <p>1</p>
    <button
      v-on:click="add(1)"
      v-bind:class="{ activeButton: floor.includes(1) }"
    >
      &otimes;
    </button>
  </div>
  <div class="lift" ref="lift" :class="{ blink: isWaiting }">
    <p
      v-bind:class="{
        floorIndicatorUp: floor[0] > activeFloor,
        floorIndicatorDown: floor[0] < activeFloor,
        floorIndicator: floor[0],
      }"
    >
      {{ floor[0] }}
    </p>
  </div>
</template>

<style scoped>
.lt {
  height: 150px;
  width: 100px;
  outline: 1px solid;
}

button {
  height: fit-content;
  margin-left: 2%;
  margin-top: 5%;
  padding: 5px;
}
.activeButton {
  background-color: blueviolet;
}
.floor {
  display: flex;
}
p {
  margin-left: 20px;
}
.floorIndicator {
  margin-top: 5%;
  margin-left: 23%;
  width: 50px;
  height: 20px;
  background-color: grey;
  text-align: center;
  border-radius: 40px;
}
.floorIndicatorUp::after {
  content: "\2191";
}
.floorIndicatorDown::after {
  content: "\2193";
}
.lift {
  background-color: aqua;
  width: 100px;
  height: 150px;
  position: absolute;
  top: 608px;
  left: 8px;
  transition-property: all;
  transition-timing-function: ease-in-out;
  transition-delay: 0s;
}
.blink {
  animation-name: blinker;
  animation-iteration-count: infinite;
  animation-timing-function: cubic-bezier(1, 2, 0, 1);
  animation-duration: 1s;
}
@keyframes blinker {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>
