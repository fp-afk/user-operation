<template>
  <div class="hello">
    <h2>用户信息</h2>
    <div class="title">
      <div>
        <el-input placeholder="请输入姓名" v-model="input2">
          <el-button slot="append" icon="el-icon-search" @click="search()"></el-button>
        </el-input>
      </div>
      <el-button type="primary" plain @click="addUser()">新增用户</el-button>
    </div>
    <el-table
    :data="tableData"
    style="width: 100%">
    <el-table-column
      prop="name"
      label="姓名"
      width="180">
    </el-table-column>
    <el-table-column
      prop="age"
      label="年龄"
      width="180">
    </el-table-column>
    <el-table-column
      prop="address"
      label="地址"
      width="260">
    </el-table-column>
    <el-table-column label="操作" width="240">
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <el-dialog :title="dialogTitle" :visible.sync="dialogFormVisible" @close="cancal()">
    <el-form :model="form">
      <el-form-item label="姓名" :label-width="formLabelWidth">
        <el-input v-model="form.name" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="年龄" :label-width="formLabelWidth">
        <el-input v-model="form.age" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="地址" :label-width="formLabelWidth">
        <el-input v-model="form.address" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="cancal()">取 消</el-button>
      <el-button type="primary" @click="sure(form)">确 定</el-button>
    </div>
  </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
      return {
        input2: '',
        // 相当数据库的值
        tableDataFixed: [{
          id: 1,
          name: '王小虎',
          age: 22,
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          id: 2,
          name: '王小虎',
          age: 18,
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          id: 3,
          name: '王小虎',
          age: 19,
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          id: 4,
          name: '王小虎',
          age: 20,
          address: '上海市普陀区金沙江路 1518 弄'
        }],
        // 可以显示模糊查询后的数据
        tableData: [{
          id: 1,
          name: '王小虎',
          age: 22,
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          id: 2,
          name: '王小虎',
          age: 18,
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          id: 3,
          name: '王小虎',
          age: 19,
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          id: 4,
          name: '王小虎',
          age: 20,
          address: '上海市普陀区金沙江路 1518 弄'
        }],
        dialogTitle: '编辑信息',
        form: {
          id: 0,
          name: '',
          age: '',
          address: '',
          index:0,
        },
        formLabelWidth: '120px',
        dialogFormVisible: false
      }
    },
    methods: {
      handleEdit(index, row) {
        this.dialogFormVisible = true;
        const data = {
          id: row.id,
          name: row.name,
          age: row.age,
          address: row.address,
          index:index,
        };
        this.form = data;
      },
      handleDelete(index, row) {
        console.log(index, row);
        this.tableData.splice(index,1);
      },
      sure(value){
        if(value.id){
          this.tableData.splice(value.index,1,value);
          this.tableDataFixed.splice(value.index,1,value);
        } else{
          const data = {
            id: this.tableData.length,
            name: value.name,
            age: value.age,
            address: value.address,
          }
          this.tableData.push(data);
          this.tableDataFixed.push(data);
        }
        
        this.dialogFormVisible = false;
        this.form={};
      },
      cancal(){
        this.dialogFormVisible = false;
        this.form={};
      },
      addUser(){
        this.dialogFormVisible = true;
        this.dialogTitle = '添加用户';

      },
      search() {
        var list = [];
        var _this = this;
        this.tableDataFixed.map((item)=>{
          if (item.name.indexOf(_this.input2) > -1) {
            return list.push(item);
          }
        });
        this.tableData = list;
      }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title{
  display: flex;
  justify-content: space-around;
}
</style>
