<template>
<div>
  <el-form :inline="true" :model="formInline" class="demo-form-inline">
  <el-form-item label="姓名">
    <el-input v-model="formInline.name" clearable></el-input>
  </el-form-item>
  <el-form-item label="性别">
    <el-select v-model="formInline.gender" clearable >
      <el-option label="男" :value="0"></el-option>
      <el-option label="女" :value="1"></el-option>
    </el-select>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="onSubmit">查询</el-button>
  </el-form-item>
</el-form>
  <el-button @click="handleAdd" type="primary">新增</el-button>
   <el-table v-loading="loading" :data="tableData" border row-key="id" style="width: 100%">
    <el-table-column prop="name" label="姓名" width="180"></el-table-column>
    <el-table-column prop="age" label="年龄" width="180"></el-table-column>
    <el-table-column prop="gender" label="性别">
       <template slot-scope="scope">
        <span v-if="scope.row.gender===0" type="text" size="small">男</span>
        <span v-if="scope.row.gender===1" type="text" size="small">女</span>
      </template>
    </el-table-column>
    <el-table-column
      label="操作">
       <template slot-scope="scope">
        <el-button @click="handleEdit(scope.row.id)" type="text" size="small">编辑</el-button>
        <el-button type="text" size="small" @click="handleDelete(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <add ref="add" @addStudent="addStudent" @editStudent="editStudent"></add>
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
          name: '',
          gender: '',
          age:''
        }
}
},created(){
  this.getData()
},methods:{
   onSubmit() {
    this.getData({...this.formInline})
      },
      //删
  handleDelete(id){
let res = JSON.parse(localStorage.list)
res=res.filter(item=>item.id!=id)
localStorage.list=JSON.stringify(res)
      this.getData()
  },
  handleAdd(){
    this.$refs.add.handleOpen()
    this.$refs.add.handleClearForm()
  },
   getStudentById (id) {
    const arr = JSON.parse(localStorage.list)
    const data = arr.filter(item => item.id === id)[0]
    return data
  },
  handleEdit(id){
    const data = this.getStudentById(id)
    this.$refs.add.handleOpen()
    this.$refs.add.setForm(data)
  },
  //改
  editStudent(value){
let arr = JSON.parse(localStorage.list)
    arr=arr.map(item=>{
      if(item.id===value.id){
        item=value
      }
      return item
    })
    localStorage.list = JSON.stringify(arr)
    this.getData()
  },
  //增
  addStudent(value){
    const arr = JSON.parse(localStorage.list)
    let id=0
    arr.forEach(item=>{
      if(item.id>id) id=item.id
    })
    arr.push({...value,id:id+1})
    localStorage.list = JSON.stringify(arr)
    this.getData()
  },
  //查
 async getData(value){
    if (!localStorage.list) {
      localStorage.list = JSON.stringify([{ id: 1, name: 'lwp', age: 24,gender:0 }])
    }
      let res = JSON.parse(localStorage.list)
      if(value){
        console.log(value)
if (value.name) {
        res = res.filter(item => item.name === value.name)
      }
      if(value.gender!==""){
        res = res.filter(item =>item.gender === value.gender)
      }
      }
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