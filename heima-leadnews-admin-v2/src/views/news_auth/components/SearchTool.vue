<template>
  <section class="filter">
    <div class="filter-container">
      <!-- TODO: 是否需要添加检索条件 -->
      <el-form ref="form" :inline="true">
        <el-form-item label="搜索内容：">
          <el-input
            v-model="title"
            placeholder="请输入内容标题"
            class="filter-item"
            clearable
            @change="queryData"
          />
        </el-form-item>
        <el-form-item label="选择类型：">
          <el-radio-group v-model="status" @change="changeStatus">
            <el-radio v-for="(item, index) in statusList" :key="index" :label="item.label">{{ item.value }}</el-radio>
          </el-radio-group>
        </el-form-item>
      </el-form>
    </div>
  </section>
</template>

<script>
export default {
  props: ['statusList','changeParam'],
  data () {
    return {
      title: '',
      status: ''
      
    }
  },
  methods: {
    queryData () {
      const params = {}
      params.title = this.title
      if (this.status !== '') {
        params.status = this.status
      }
      this.changeParam(params)
    },
    // 切换状态
    changeStatus () {
      this.queryData() // 查询数据
    }
  }
  
  
}
</script>
