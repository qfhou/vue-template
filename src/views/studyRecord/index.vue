<template>
  <div class="dashboard-container">
    <div class="filter-container">
      <el-row>
        <el-col :span="8">
          <div class="grid-content" />
        </el-col>
        <el-col :span="8">
          <div class="grid-content">
            <div class="filter-item">
              <el-input
                v-model="queryTitle"
                placeholder="培训名称"
                style="width: 200px;margin-right: 15px;"
                class="filter-item"
                @keyup.enter.native="handleFilter"
              />
              <el-button class="filter-item" type="primary" icon="el-icon-search" @click.native="handleFilter">
                查询
              </el-button>
            </div>
          </div>
        </el-col>
        <el-col :span="8">
          <div class="grid-content" />
        </el-col>
      </el-row>
    </div>
    <el-table
      v-loading="listLoading"
      border
      element-loading-text="Loading"
      :data="list"
      :current-page.sync="currentPage"
    >
      <el-table-column label="序号" type="index" width="50" align="center" />
      <!-- <el-table-column label="序号" width="100">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column> -->
      <el-table-column label="培训名称" width="400">
        <template slot-scope="scope">
          {{ scope.row.trainingName }}
        </template>
      </el-table-column>
      <el-table-column label="学习时长(分钟)" width="200" align="center">
        <template slot-scope="scope">
          {{ scope.row.duration }}
        </template>
      </el-table-column>
      <el-table-column prop="modifyTime" label="最近学习时间" width="auto" align="center" />
      <el-table-column prop="grade" label="考试分数" width="auto" align="center" />
      <el-table-column prop="recordStatusName" label="考核状态" width="auto" align="center" />
    </el-table>
    <div class="pagination-container">
      <el-pagination :page-sizes="[5, 9, 15]" layout="total, sizes, prev, pager, next, jumper" :total="total" @size-change="handleSizeChange" @current-change="handleCurrentChange" />
    </div>
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
      listLoading: true,
      stripe: true,
      tableData: [],
      currentPage: 1,
      pagesize: 5,
      total: 0,
      item: null,
      queryTitle: null
    }
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  created() {
    this.GetList({ title: this.queryTitle, pageSize: this.pagesize, page: this.currentPage })
  },
  methods: {
    GetList(params) {
      request({
        url: '/rest/student/study/archives',
        method: 'get',
        params
      }).then(response => {
        this.list = response.data.data?.list
        this.total = response.data.data?.total
        this.listLoading = false
      })
    },
    handleFilter() {
      this.GetList({ trainingName: this.queryTitle, pageSize: this.pagesize, page: this.currentPage })
    },
    handleSizeChange(val) {
      this.pagesize = val
    },
    handleCurrentChange(val) {
      this.currentPage = val
      this.handleFilter()
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
