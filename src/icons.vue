<template>
  <div class="icon-list">
    <div v-for="icon in locIcons" :key="icon.id" class="icon-item">
      <span class="icon-example" :class="[icon.fontClass]"></span>
      <div class="icon-name">{{ icon.name }}</div>
      <div class="icon-cover" @click="handlerCopy(icon)">
        <div v-if="!isActive(icon)" class="icon-cover-text">复制代码</div>
        <div v-else class="icon-cover-text">复制成功</div>
      </div>
    </div>
  </div>
</template>

<script>
import icons from "../lib/config.json";
import "../lib/index.min.css";
export default {
  props: {
    query: {
      type: String,
      default: "",
    },
  },
  computed: {
    locIcons() {
      if (this.query) {
        return icons.filter((item) => item?.name?.includes(this.query));
      } else {
        return icons;
      }
    },
  },
  data() {
    return {
      activeIcon: "",
      timer: null,
    };
  },
  methods: {
    isActive(icon) {
      return icon.fontClass === this.activeIcon;
    },
    handlerCopy(icon) {
      this.activeIcon = icon?.fontClass;
      if (this.timer) clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.activeIcon = "";
      }, 800);
      const copyValue = `<span class='${icon.fontClass}'></span>`;
      if (navigator.clipboard) {
        navigator.clipboard.writeText(copyValue);
      } else {
        const input = document.createElement("input");
        input.setAttribute("readonly", "readonly");
        input.style.position = "absolute";
        input.style.clip = "rect(0 0 0 0)";
        input.value = copyValue;
        document.body.appendChild(input);
        input.select();
        if (typeof document.execCommand === "function") {
          document.execCommand("copy", true);
        }
        document.body.removeChild(input);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.icon-list {
  margin: 20px 0;
  width: 100% !important;
  overflow: hidden;
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  border-left: 1px solid #eaeefb;
  border-top: 1px solid #eaeefb;
}
.icon-item {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  cursor: pointer;
  border-right: 1px solid #eee;
  border-bottom: 1px solid #eee;
  width: 12.5%;
  overflow: hidden;
  &:hover {
    .icon-cover {
      display: flex;
    }
  }
}
.icon-cover {
  width: 100%;
  height: 100%;
  background: rgba(13, 10, 49, 0.8);
  position: absolute;
  top: 0;
  left: 0;
  z-index: 100;
  overflow: hidden;
  vertical-align: middle;
  border-radius: 5px;
  color: #fff;
  font-size: 12px;
  font-weight: bold;
  display: none;
  align-items: center;
  justify-content: center;
  transition: visibility 0.25s linear;
}
.icon-example {
  font-size: 30px;
  color: #333;
  height: 90px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: font-size 0.25s linear, width 0.25s linear;
}
.icon-name {
  font-size: 13px;
  color: #99a9bf;
  height: 30px;
  line-height: 30px;
  overflow: hidden;
  padding: 0 8px;
  text-overflow: ellipsis;
  white-space: nowrap;
  text-overflow: ellipsis;
  width: 100%;
  text-align: center;
}
</style>
