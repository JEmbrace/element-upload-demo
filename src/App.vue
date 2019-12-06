<template>
  <div id="app">
    <!-- element-ui Table表格组件 -->
    <el-table
        class="my-table"
        :data="tableData"
        stripe
        style="width:725px;">
        <el-table-column
          prop="date"
          label="日期"
          width="180">
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名"
          width="180">
        </el-table-column>
        <el-table-column
          prop="address"
          label="地址"
          width="180">
        </el-table-column>
        <!-- 添加一列附件管理（文件上传） -->
        <el-table-column
          prop="attach"
          label="附件管理"
          width="180">
          <template>
            <!-- 上传按钮绑定click事件 -->
            <el-button size='small' type="primary" @click="dialogVisible = true">上传<i class="el-icon-upload el-icon--right"></i></el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-dialog
        title="提示"
        :visible.sync="dialogVisible"
        width="30%">
        <span>这是一段信息</span>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
        </span>
      </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      // 添加属性，默认值为false,表示弹框不显示
      dialogVisible: false,
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路'
      }]
    } 
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 50px 30px;
  text-align: center;
}
</style>
<style scoped>
#app .my-table{
  display: inline-block;
  border: 1px solid #EBEEF5;
}
</style>

