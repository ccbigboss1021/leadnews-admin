<template>
  <div>
    <search-tool :changeParam="changeParam"  :statusList="statusList" />
    <search-result
      ref="mySearchResult"
      :list="list"
      :total="total"
      :changePage="changePage"
      :submitSuccess="submitSuccess"
      :pageSize="params.size"
    />
  </div>
</template>

<script>
import SearchTool from './components/SearchTool.vue'
import SearchResult from './components/SearchResult.vue'
import { authList } from '@/api/news_auth'
export default {
  name: 'ChannelManager',
  data () {
    return {
      params: {
        page: 1,
        size: 10,
        title: '',
        status: ''
      },
      total: 0,
      list: [],
       statusList: [
        { label: '', value: '全部' },
        { label: 3, value: '待人工审核' },
        { label: 4, value: '人工审核通过' },
        { label: 2, value: '审核失败' },
        { label: 9, value: '已发布' }
      ],
    }
  },
  mounted () {
    this.loadData()
  },
  components: { SearchTool, SearchResult },
  methods: {
    submitSuccess: function () {
      this.loadData()
    },
    changeParam: function (obj) {
      this.params.page = 1
      this.params.title = obj.title
      this.params.status = obj.status
      this.loadData()
    },
    changePage: function (e) {
      this.params.page = e.page
      this.loadData()
    },
    async loadData () {
      const res = await authList({ ...this.params })
      if (res.code === 200) {
        this.list = res.data
        this.total = res.total
      } else {
        this.$message({ type: 'error', message: res.error_message })
      }
    }
  }
}
</script>
