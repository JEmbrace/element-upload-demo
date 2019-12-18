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
          <template slot-scope="scope">
            <!-- 上传按钮绑定click事件 -->
            <el-button size='small' type="primary" @click="uploadBtnClick(scope.$index)">
              上传
              <i class="el-icon-upload el-icon--right"></i>
            </el-button>          
          </template>
        </el-table-column>
      </el-table>
      <el-dialog
        title="附件管理"
        :visible.sync="dialogVisible"
        width="30%">
          <!-- 将<el-upload>代码添加到<el-dialog>代码块中 -->
          <el-upload
            class="upload-demo"
            drag
            action="https://jsonplaceholder.typicode.com/posts/"
            :on-remove="handleRemove"
            :before-remove="beforeRemove"
            :file-list="currentAttachList"
            :on-success="uploadSuccess"
            :before-upload="beforeUpload">
            <i class="el-icon-upload"></i>
            <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
            <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div>
          </el-upload>
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
     // 设置当前文件列表数据currentAttachList，每次用户点击上传按钮，该数据就会被赋值为当前按钮那一列tableData中的attachList数据
      currentAttachList: [],
      //当前点击打开弹框的按钮在表格中是那一列
      currentIndex: 0,
      //是否包含重复的文件名称,默认不包含值为false
      isRepeat: false,
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路',
        attachList:[]
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路',
        attachList:[]
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路',
        attachList:[]
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路',
        attachList:[]
      }]
    } 
  },
  methods: {
    uploadBtnClick (index){
      // 获取上传按钮对应那一列表格数据中的附件列表，赋值给currentAttachList
      this.currentAttachList = this.tableData[index].attachList;
      // 将控制弹框显示的dialogVisible设置为true，让弹框显示
      this.dialogVisible = true;
      // 设置currentIndex
      this.currentIndex = index;
    },
    uploadSuccess(response, file, fileList){
      var currentIndex = this.currentIndex;
      this.tableData[currentIndex].attachList.push({
        'name':file.name
      });
    },
    beforeRemove(file, fileList){
      if(this.isRepeat == false){
        return this.$confirm('此操作将永久删除' + file.name +'文件, 是否继续?');
      }
    },
    handleRemove(file, fileList){
      if(this.isRepeat == false){
        var currentIndex = this.currentIndex;
        var attachList = this.tableData[currentIndex].attachList;
        var tempList = [];
        for(var i = 0; i<attachList.length; i++){
          if(file.name != attachList[i].name){
            tempList.push(attachList[i]);
          }
        }
        this.tableData[currentIndex].attachList = tempList;
      }else{
        this.isRepeat = false;
      }
    },
    beforeUpload(file){
      var currentIndex = this.currentIndex;
      //首先需要获取当前已经上传的文件列表
      var list = this.tableData[currentIndex].attachList;
      //循环文件列表判断是否有重复的文件
      for(var i = 0;i<list.length;i++){
        if(list[i].name == file.name){
          this.$message.error(file.name + '文件名重复');
          //添加逻辑：得知上传了重复文件后，设置一个标志值为true，提供给beforeRemove函数使用
          this.isRepeat = true;
          //记得一定要返回false,否则控件继续会执行上传操作
          return false;
        }
      }
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
#app .el-dialog__header{
  background:#EBEEF5;
  border-bottom: 1px solid#EBEEF5;
}
#app .el-dialog{
  text-align: left;
}
#app .el-upload,#app .el-upload .el-upload-dragger{
  width: 100%;
}
#app .el-upload-list li{
  transition: none;
}
</style>
<style scoped>
#app .my-table{
  display: inline-block;
  border: 1px solid #EBEEF5;
}
</style>
