<style scoped>
.cs-image {
  overflow: hidden;
}

.cs-image--img {
  width: 50px;
  height: 50px;
}
</style>
<template>
<div class="cs-image" :style="getStyle">
  <image @load="onLoad" class="cs-image--img" :style="getStyle" ref="img" :src="src" @click="handleClick" />
</div>
</template>

<script>
/**
 * Name
 * 读取图片 - 可自动获取图片实际大小，可自定义
 *
 * Prop
 * src                图片地址
 * scale              缩放比例
 * width              宽度（type=custom生效）
 * height             高度（type=custom生效）
 * backgroundColor    背景颜色
 * imgStyle           自定义样式style
 * type               custom, normal
 */
export default {
  data: () => ({
    loaded: false,
    imgWidth: 0,
    imgHeight: 0
  }),
  created() {
    const {
      width,
      height
    } = this;
    this.imgWidth = width;
    this.imgHeight = height;
  },
  watch: {
    width(val) {
      this.imgWidth = val;
    },
    height(val) {
      this.imgHeight = val;
    }
  },
  props: {
    src: {
      type: String
    },
    scale: {
      type: Number,
      default: 1
    },
    width: {
      type: Number
    },
    height: {
      type: Number
    },
    backgroundColor: {
      type: String,
      default: "transparent"
    },
    imgStyle: {
      type: Object
    },
    type: {
      type: String,
      default: "normal"
    }
  },
  computed: {
    getStyle() {
      const {
        imgWidth,
        imgHeight,
        type
      } = this;
      return {
        width: `${imgWidth}px`,
        height: `${imgHeight}px`,
        backgroundColor: this.loaded ? "transparent" : this.backgroundColor,
        ...this.imgStyle
      };
    }
  },
  methods: {
    onLoad(e) {
      const {
        scene
      } = this;
      e.success && (this.loaded = true);
      if (e.success && e.size && e.size.naturalWidth > 0 && this.scale) {
        if (this.type === "normal") {
          this.imgWidth = e.size.naturalWidth * this.scale;
          this.imgHeight = e.size.naturalHeight * this.scale;
        } else if (this.type === "custom") {
          this.imgWidth = this.width;
          this.imgHeight = this.height;
        }
      }
    },
    handleClick(e) {
      this.$emit("click", e);
    }
  }
};
</script>
