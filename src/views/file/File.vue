<template>
  <div class="file-wrapper">
    <el-container class="el-container">
      <el-aside width="auto">
        <AsideMenu></AsideMenu>
      </el-aside>
      <el-container>
        <el-main class="el-main">
          <FileList></FileList>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import AsideMenu from './components/AsideMenu/AsideMenu'
import FileList from './components/FileList'

export default {
  name: 'File',
  components: {
    AsideMenu,
    FileList
  }
}
</script>

<style lang="stylus" scoped>
@import '~@/assets/styles/varibles.styl'
@import '~@/assets/styles/mixins.styl'
.file-wrapper
  width 100% !important
.el-container
  box-sizing: border-box;
  .el-aside
    border-right solid 1px $BorderLight
    background  $Info
    height calc(100vh - 61px)
    setScrollbar(4px)
.el-main
  padding-top 0px !important
  overflow hidden
</style>