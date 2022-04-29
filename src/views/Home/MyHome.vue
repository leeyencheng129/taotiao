<template>
  <div class="home-container">
    <van-nav-bar title="标题" fixed />

    <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="onLoad">
      <ArticleInfo v-for="item in artList" :key="item.id" :title="item.title" :author="item.aut_name" :cmtCount="item.comm_count" :time="item.pubdate" :cover="item.cover"></ArticleInfo>
    </van-list>
  </div>
</template>

<script>
import { getArticleListAPI } from '@/api/articleAPI.js'
// const result = getArticleListAPI(1, 5)
// console.log(result)
import ArticleInfo from '@/components/Article/ArticleInfo.vue'

export default {
  name: 'MyHome',
  data () {
    return {
      page: 1,
      limit: 10,
      artList: [],
      loading: true,
      finished: false
    }
  },
  created () {
    this.initArticleList()
  },
  methods: {
    async initArticleList () {
      const { data: res } = await getArticleListAPI(this.page, this.limit)
      // this.artList = res
      this.artList = [...this.artList, ...res]
      this.loading = false

      if (res.length === 0) {
        this.finished = true
      }
    },
    onLoad () {
      console.log('觸發')
      this.page++
      this.initArticleList()
    }
  },
  components: {
    ArticleInfo
  }
}
</script>

<style lang="scss" scoped>
.home-container {
  padding: 50px 0;
  ::v-deep .van-nav-bar {
    background: rgb(23, 216, 174);
    .van-nav-bar__title {
      color: #fff;
    }
  }
}
</style>
