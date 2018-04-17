
<style lang="css" scoped>
.cs-status__navBar {
  background-color: yellow;
}
</style>
<template>
<div class="cs-status__navBar" :style="{'height': statusBarHeight}">
</div>
</template>
<script>
/**
 * Name
 * eros --- 获取通知栏的高度,用户自定义导航条的页面
 */
export default {
  data: () => ({
    statusBarHeight: weex.config.eros.statusBarHeight ?
      weex.config.eros.statusBarHeight : 40
  })
};
</script>
