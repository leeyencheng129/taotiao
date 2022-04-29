<template>
  <div>
    <van-cell>
      <!-- 標題專區的插槽 -->
      <template #title>
        <div class="title-box">
          <!-- 標題 -->
          <span>{{title}}</span>
          <!-- 單張圖片 -->
          <img :src="cover.images[0]" class="thumb" v-if="cover.type === 1">
        </div>
        <!-- 三張圖片 -->
        <div class="thumb-box" v-if="cover.type !== 1">
          <img :src="item" class="thumb" v-for="(item,i) in cover.images" :key="i">

        </div>
      </template>
      <!-- label-box區塊的插槽 -->
      <template #label>
        <div class="label-box">
          <span>作者 {{author}}&nbsp;&nbsp; {{cmtCount}}評論 發布日期 {{time}}</span>
          <!-- 關閉按鈕 -->
          <van-icon name="cross" />
        </div>
      </template>
    </van-cell>
  </div>
</template>

<script>
export default {
  name: 'ArticleInfo',
  props: {
    title: {
      type: String,
      default: ''
    },
    author: {
      type: String,
      default: ''
    },
    cmtCount: {
      // 兩種類型都可以
      type: [Number, String],
      default: 0
    },
    time: {
      type: String,
      default: ''
    },
    cover: {
      type: Object,
      default: function () {
        return { type: 0 }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.label-box {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.thumb {
  width: 113px;
  height: 70px;
  background-color: #f8f8f8;
  object-fit: cover;
}
.title-box {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
}
.thumb-box {
  display: flex;
  justify-content: space-between;
}
</style>
