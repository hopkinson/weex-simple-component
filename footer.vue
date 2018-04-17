<!-- 固定在底部 -->
<template>
<div class="c-footer" :style="setStyle">
  <slot></slot>
</div>
</template>

<style scoped>
.c-footer {
  flex-direction: row;
  position: fixed;
  right: 0;
  bottom: 0;
  left: 0;
  justify-content: center;
  align-items: center;
  padding: 0 48px;
}
</style>

<script>
/**
 * Name
 * 元素固定在底部
 *
 * props
 * backgroundColor   背景色
 * height            高度
 */
export default {
  computed: {
    setStyle() {
      return {
        backgroundColor: this.backgroundColor,
        height: this.height
      }
    }
  },
  components: {

  },
  props: {
    backgroundColor: {
      type: String,
      default: 'transparent'
    },
    height: {
      type: [String, Number]
      default: '144'
    }
  }
};
</script>
