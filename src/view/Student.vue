<template>
<div>
  <el-form :inline="true" :model="formInline" class="demo-form-inline">
  <el-form-item label="姓名">
    <el-input v-model="formInline.name"></el-input>
  </el-form-item>
  <el-form-item label="性别">
    <el-select v-model="formInline.gender">
      <el-option label="男" value="0"></el-option>
      <el-option label="女" value="1"></el-option>
    </el-select>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="onSubmit">查询</el-button>
  </el-form-item>
</el-form>
  <el-button @click="addStudent" type="primary">新增</el-button>

   <el-table
   v-loading="loading"
    :data="tableData"
    border
    row-key="id"
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
      prop="gender"
      label="性别">
         {{scope}}
       <template slot-scope="scope">
        <span v-if="scope.row.gender===0" type="text" size="small">{{scope.$text}}男</span>
        <span v-if="scope.row.gender===1" type="text" size="small">{{scope.row.gender}}女</span>
      </template>
    </el-table-column>
    <el-table-column
      label="操作">
       <template slot-scope="scope">
        <el-button @click="handleClick(scope.row)" type="text" size="small">编辑</el-button>
        <el-button type="text" size="small" @click="handleDelete(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <add></add>
</div>
</template>
<script>
const sleep = (time) => {
    return new Promise((resolve) => {
      setTimeout(resolve, time);
    })
  }
  import Add from './components/Add'
export default {
data(){
return {
  tableData: [],
        loading:false,
        formInline: {
          user: '',
          region: ''
        }
}
},created(){
  this.getData()
},methods:{
   onSubmit() {
        console.log('submit!');
      },
  handleDelete(id){
let res = JSON.parse(localStorage.list)
res=res.filter(item=>item.id!=id)
localStorage.list=JSON.stringify(res)
      this.tableData=res
  },
  addStudent(){
// let res = JSON.parse(localStorage.list)

  },
 async getData(){
    if (!localStorage.list) {
      localStorage.list = JSON.stringify([{ id: 1, name: 'lwp', age: 24,gender:0 }])
    }
      let res = JSON.parse(localStorage.list)
      this.loading=true
      await sleep(500)
      this.loading=false
      this.tableData=res
  }
},components:{
  Add
}
}
</script>
<style lang='less' scoped>
</style>