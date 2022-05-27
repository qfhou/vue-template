<template>
  <div class="dashboard-container">
    <el-table
      v-loading="listLoading"
      border
      element-loading-text="Loading"
      :data="list"
    >
      <el-table-column label="序号" type="index" width="50" align="center" />
      <el-table-column label="培训名称" align="center">
        <template slot-scope="scope">
          {{ scope.row.name }}
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center" width="230" class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button type="primary" @click.native="showDetail(scope.row)">去学习</el-button>
          <el-button type="primary" @click.native="showDetail(scope.row)">去考试</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
import { mapGetters } from 'vuex'
import request from '@/utils/request'
// import { Message } from 'element-ui'

export default {
  name: 'Dashboard',
  data() {
    return {
      list: null,
      listLoading: true
    }
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  created() {
    this.GetList('')
  },
  methods: {
    GetList(params) {
      request({
        url: '/rest/training/mine',
        method: 'get',
        params
      }).then(response => {
        this.list = response.data.data
        this.listLoading = false
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }

  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}

.showOverTooltip {
  display: -webkit-box;
  text-overflow: ellipsis;
  overflow: hidden;
  /*这里是3行*/
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.filter-container {
  padding-bottom: 10px;

  .filter-item {
    display: inline-block;
    vertical-align: middle;
    margin-bottom: 10px;
  }
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}

.pagination-container {
  background: #fff;
  padding: 32px 16px;
}
</style>
