<template>
  <div class="home-container">
    <van-nav-bar title="標題" fixed />
    <van-pull-refresh v-model="isLoading" :disabled="finished" success-text="刷新成功" @refresh="onRefresh">
      <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="onLoad">
        <ArticleInfo
        v-for="item in artList"
        :key="item.id" :title="item.title"
        :author="item.aut_name"
        :cmtCount="item.comm_count"
        :time="item.pubdate"
        :cover="item.cover">
        </ArticleInfo>
      </van-list>
    </van-pull-refresh>
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
      finished: false,
      isLoading: false
    }
  },
  created () {
    this.initArticleList()
  },
  methods: {
    async initArticleList (isRefresh) {
      const { data: res } = await getArticleListAPI(this.page, this.limit)
      // this.artList = res
      if (isRefresh) {
        // 如果是true，就讓上面加載新資料
        this.artList = [...res, ...this.artList]
        this.isLoading = false
      } else {
        // 如果是沒有true，就會變成undefinde，就讓上面加載新資料
        this.artList = [...this.artList, ...res]
        this.loading = false
      }

      if (res.length === 0) {
        this.finished = true
      }
    },
    onLoad () {
      console.log('觸發')
      this.page++
      this.initArticleList()
    },
    onRefresh () {
      console.log('觸發下拉刷新')
      this.page++
      this.initArticleList(true)
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
    // background: rgb(23, 216, 174);
    .van-nav-bar__title {
      color: #fff;
    }
  }
}
</style>
