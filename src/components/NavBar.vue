<template>
  <div>
    <div class="menu-container" @mouseover="mouseEvent($event, 'wrapper')">
      <div class="logo">
        <a href="https://tomaraei.com">
          <img src="../assets/logo.svg" />
        </a>
      </div>
      <div class="items">
        <a
          v-for="item in menuItems"
          @mouseover="mouseEvent($event, 'item', item.key)"
          :key="item.key"
          class="item"
        >
          {{ item.label }}
        </a>
      </div>
      <div class="icons">
        <div class="icon">
          <img src="../assets/magnifier.svg" />
        </div>
        <div class="icon">
          <img src="../assets/users.svg" />
        </div>
        <div class="icon menu">
          <img src="../assets/menu.svg" />
        </div>
      </div>
    </div>
    <MegaMenu v-if="showMegaMenu" />
    <div class="viewport-warning">
      <div class="message">
        This example was made for viewport sizes 920px and above :)
      </div>
    </div>
  </div>
</template>

<script>
import MegaMenu from "./MegaMenu";
import eventBus from "../eventBus";

export default {
  name: "NavBar",
  components: {
    MegaMenu,
  },
  data() {
    return {
      menuItems: [
        { key: "products", label: "Products" },
        { key: "solutions", label: "Solutions" },
        { key: "pricing", label: "Pricing" },
        { key: "case-studies", label: "Case Studies" },
        { key: "blog", label: "Blog" },
        { key: "contact", label: "Contact" },
      ],
      showMegaMenu: false,
    };
  },
  methods: {
    mouseEvent(event, source, key = "") {
      if (source === "item") {
        event.stopPropagation();
      }
      this.showMegaMenu = key === "products";
    },
  },
  mounted() {
    eventBus.$on("hide-mega-menu", () => {
      this.showMegaMenu = false;
    });
  },
  beforeDestroy() {
    eventBus.$off("hide-mega-menu");
  },
};
</script>

<style scoped lang="scss">
.menu-container {
  display: flex;
  justify-content: space-between;
  height: 77px;
  align-items: center;
  border-bottom: 1px solid #a4a4a4;
}

.logo {
  display: flex;
  height: 33px;
  cursor: pointer;
  padding-left: 30px;
}

.icons {
  display: inline-flex;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.icon {
  cursor: pointer;
  display: flex;
  padding: 22px 30px;
}

.icon:hover {
  background: #efefef;
}

.icon.menu {
  @media (min-width: 921px) {
    display: none;
  }
}

.items {
  display: inline-flex;
  height: 101%;
  overflow: hidden;
  @media (max-width: 920px) {
    display: none;
  }
}

.item {
  cursor: pointer;
  height: 100%;
  align-items: center;
  height: 100%;
  display: flex;
  padding: 0 15px;
}

.item:hover {
  background: #efefef;
}

.viewport-warning {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  background: rgba(255, 255, 255, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  font-weight: bold;
  @media (min-width: 921px) {
    display: none;
  }

  .message {
    padding: 15px;
  }
}
</style>
