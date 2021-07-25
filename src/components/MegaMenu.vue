<template>
  <div @mouseleave="hideMegaMenu()" class="megamenu-wrapper">
    <div class="main-categories">
      <div
        v-for="index in 8"
        :key="index"
        @mouseover="changeMainCategory(index)"
        :class="['main-category', isActive(index) ? 'active' : '']"
      >
        <div class="icon"><img src="../assets/main-category.svg" /></div>
        <div class="label">Main category {{ index }}</div>
      </div>
    </div>
    <div class="sub-categories">
      <div v-for="index in 15" :key="index" class="sub-category">
        <div class="icon"><img src="../assets/sub-category.svg" /></div>
        <div class="label">
          Sub-category {{ activeMainCategory }}/{{ index }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import eventBus from "../eventBus";

export default {
  name: "MegaMenu",
  data() {
    return {
      activeMainCategory: 1,
      lastX: 0,
      lastY: 0,
      direction: "",
      throttle: false,
    };
  },
  methods: {
    hideMegaMenu() {
      eventBus.$emit("hide-mega-menu");
    },
    isActive(key) {
      return this.activeMainCategory === key;
    },
    mouseDirection(e) {
      if (!this.throttle) {
        this.throttle = true;
        let theta = Math.abs(
          (180 * Math.atan2(e.pageY - this.lastY, e.pageX - this.lastX)) /
            Math.PI
        );
        this.direction = theta > 75 ? "vertical" : "horizontal";
        this.lastX = e.pageX;
        this.lastY = e.pageY;
        setTimeout(() => {
          this.throttle = false;
        }, 50);
      }
    },
    changeMainCategory(index) {
      if (this.direction === "vertical") {
        this.activeMainCategory = index;
      }
    },
  },
  mounted() {
    window.addEventListener("mousemove", this.mouseDirection);
  },
  beforeDestroy() {
    window.removeEventListener("mousemove", this.mouseDirection);
  },
};
</script>

<style scoped>
.megamenu-wrapper {
  border: 1px solid #a4a4a4;
  border-top: none;
  max-width: 1120px;
  margin: 0 auto;
  height: 500px;
  background: #f8f8f8;
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-template-rows: 1fr;
  gap: 0px 0px;
  grid-template-areas: ". .";
}

.main-categories {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: auto;
  gap: 0px 0px;
  border-right: 1px solid #a4a4a4;
}

.main-category {
  display: flex;
  cursor: pointer;
  flex-direction: row;
  align-items: center;
  padding-left: 30px;
}

.main-category:not(:last-of-type) {
  border-bottom: 1px solid #a4a4a4;
}

.main-category .icon {
  margin-right: 15px;
}

.main-category.active {
  background: #efefef;
}

.sub-categories {
  background: #efefef;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr;
  gap: 0px 0px;
  grid-template-areas:
    ". . . . ."
    ". . . . ."
    ". . . . .";
}

.sub-category {
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.sub-category:hover {
  background: #dedede;
}

.sub-category .label {
  margin-top: 15px;
}
</style>
