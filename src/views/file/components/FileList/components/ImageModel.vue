<template>
  <div class="image-model-wrapper">
    <!-- 网格模式 -->
    <ul class="image-model" v-show="imageModel === 1">
      <li
        class="image-item"
        v-for="(item, index) in fileList"
        :key="index"
        @click="getImgReviewData(fileList, index)"
      >
        <img class="image" :src="downloadImgMin(item)" :alt="item.fileName + item.extendName" />
        <div class="image-name">{{item.fileName + '.' + item.extendName}}</div>
      </li>
    </ul>
    <!-- 时间线模式 -->
    <div v-show="imageModel === 2">
      <div class="radio">
        排序：
        <el-radio-group v-model="reverse">
          <el-radio :label="true">倒序</el-radio>
          <el-radio :label="false">正序</el-radio>
        </el-radio-group>
      </div>
      <el-timeline class="image-timeline" :reverse="reverse" v-if="imageTimelineData.length">
        <el-timeline-item
          class="image-item"
          v-for="(item, index) in imageTimelineData"
          :key="index"
          :timestamp="item.uploadDate"
          placement="top"
        >
          <img
            class="image"
            v-for="(image, index) in item.imageList"
            :key="image.fileid"
            :src="downloadImgMin(image)"
            :alt="image.fileName + image.extendName"
            @click="getImgReviewData(item.imageList, index)"
          />
        </el-timeline-item>
      </el-timeline>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageModel',
  props: {
    fileList: Array
  },
  data() {
    return {
      reverse: true
    }
  },
  computed: {
    imageModel() {
      return this.$store.getters.imageModel
    },
    //  按年-月-日分组排序
    imageTimelineData() {
      let res = []
      //  去重，获取返回的所有日期年-月-日
      let uploadTimeSet = new Set(
        this.fileList.map(item => item.uploadTime.split(' ')[0])
      )
      let uploadDate = [...uploadTimeSet]
      //  分组
      uploadDate.forEach(element => {
        res.push({
          uploadDate: element,
          imageList: this.fileList.filter(
            item => item.uploadTime.split(' ')[0] === element
          ) //  过滤
        })
      })
      return res
    }
  },
  methods: {
    getImgReviewData(imgList, index) {
      let data = {
        imgReviewVisible: true,
        imgReviewList: imgList.map(item => {
          return {
            fileUrl: this.getViewFilePath(item),
            downloadLink: this.getDownloadFilePath(item),
            fileName:  item.fileName,
            extendName: item.extendName
          }
        }),
        activeIndex: index,
      }
      this.$store.commit('setImgReviewData', data)
    }
  }
}
</script>

<style lang="stylus" scoped>
@import '~@/assets/styles/mixins.styl'
.image-model-wrapper
  margin-top 20px
  padding 0 20px
  height calc(100vh - 200px)
  overflow-y auto
  setScrollbar(10px)
  .image-model
    display flex
    flex-wrap wrap
    .image-item
      margin 0 10px 10px 0
      width 150px
      cursor pointer
      .image
        width 150px
        height 150px
      .image-name
        padding-top 4px
        font-size 12px
        text-align center
  .image-timeline
    margin-top 10px
    .image
      margin 0 10px 10px 0
      width 150px
      height 150px
      cursor pointer
</style>