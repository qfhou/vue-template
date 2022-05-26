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
                placeholder="公告题目"
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
      <el-table-column label="公告题目" width="400">
        <template slot-scope="scope">
          {{ scope.row.title }}
        </template>
      </el-table-column>
      <el-table-column label="公告内容" width="200" align="center">
        <template slot-scope="scope">
          <p class="showOverTooltip">{{ scope.row.content }}</p>
        </template>
      </el-table-column>
      <el-table-column prop="statusName" label="状态" width="auto" align="center" />
      <el-table-column prop="readTime" label="阅读时间" width="auto" align="center" />
      <el-table-column label="操作" width="150" align="center">
        <template slot-scope="scope">
          <el-button type="primary" @click.native="showDetail(scope.row)">查看详情</el-button>
        </template>
      </el-table-column>
      <!-- <el-table-column align="center" label="Actions" width="120">
        <template slot-scope="scope">
          <router-link :to="'/example/edit/'+scope.row.id">
            <el-button type="primary" size="small" icon="el-icon-edit">
              Edit
            </el-button>
          </router-link>
        </template>
      </el-table-column> -->
    </el-table>
    <div class="pagination-container">
      <el-pagination :page-sizes="[5, 9, 15]" layout="total, sizes, prev, pager, next, jumper" :total="total" @size-change="handleSizeChange" @current-change="handleCurrentChange" />
    </div>

    <el-dialog :visible.sync="dialogVisible" title="公告">
      <div>
        <div class="demo-input-suffix">
          公告题目：
          <span>{{ item && item.title }}</span>
        </div>
        <div class="demo-input-suffix">
          <h3>公告内容：</h3>
          <span>{{ item && item.content }}</span>
        </div>
      </div>
      <template #footer>
        <span class="dialog-footer">
          <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
        </span>
      </template>
    </el-dialog>
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
      dialogVisible: false,
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
    this.announcementList({ title: this.queryTitle, pageSize: this.pagesize, page: this.currentPage })
  },
  methods: {
    announcementList(params) {
      request({
        url: '/rest/student/record/listdata',
        method: 'get',
        params
      }).then(response => {
        this.list = response.data.data?.list
        this.total = response.data.data?.total
        this.listLoading = false
      })
    },
    handleFilter() {
      this.announcementList({ title: this.queryTitle, pageSize: this.pagesize, page: this.currentPage })
    },
    showDetail(item) {
      this.item = item
      this.dialogVisible = true
    },
    handleSizeChange(val) {
      this.pagesize = val
    },
    handleCurrentChange(val) {
      this.currentPage = val
      this.handleFilter()
    },
    handleClose(done) {
      this.$confirm('确认关闭？')
        .then(_ => {
          done()
        })
        .catch(_ => { })
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
