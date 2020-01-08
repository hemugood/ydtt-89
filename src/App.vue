<template>
  <div id="app">
    <router-view />
    <i class="iconfont icon-Apple"></i>
    <van-icon name="fire-o" />
    <van-icon name="fire-o" />
    <van-icon class-prefix="iconfont" name="icon-Apple" />
    <van-button type="default">默认按钮</van-button>
    <van-button type="primary">主要按钮</van-button>
    <van-button type="info">信息按钮</van-button>
    <van-button type="warning">警告按钮</van-button>
    <van-button type="danger">危险按钮</van-button>
    <van-cell-group>
      <van-cell title="单元格" value="内容" />
      <van-cell title="单元格" value="内容" label="描述信息" />
    </van-cell-group>

  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style lang="less" scoped>
// @import url("@/styles/variales.less");
@import url("~@/styles/variables.less");
</style>
