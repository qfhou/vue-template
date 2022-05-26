<template>
  <div class="btns">
    <button class="btn btn-default" @click="add">新增</button>
    <button class="btn btn-default" @click="edit">修改</button>
    <el-dialog :visible.sync="showDialog" title="新增编辑表单">
      <span>这是一段信息</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
  <!-- <el-dialog title="提示" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
    <span>这是一段信息</span>
    <span slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">取 消</el-button>
      <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
    </span>
  </el-dialog> -->
</template>

<script>
export default {
  data() {
    return {
      showDialog: false
    }
  },
  created() {

  },
  methods: {
    add() {
      this.showDialog = true
    },
    edit() {
      this.showDialog = true
    },
    closeDialog() {
      this.showDialog = false
    }
  }
}
</script>
