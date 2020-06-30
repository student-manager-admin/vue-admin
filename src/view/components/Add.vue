<template>
  <div>
    <el-dialog title="新增" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <el-form :model="form">
        <el-form-item label="姓名">
          <el-input v-model="form.name" style="width:280px"></el-input>
        </el-form-item>
        <el-form-item label="年龄">
          <el-input v-model="form.age" style="width:280px"></el-input>
        </el-form-item>
        <el-form-item label="性别">
          <el-select v-model="form.gender" style="width:280px">
            <el-option label="男" :value="0"></el-option>
            <el-option label="女" :value="1"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="submit">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
export default {
  data() {
    return {
      dialogVisible: false,
      title:'新增',
      man:0,
      woman:1,
      form: {
        name: "",
        gender: "",
        age: ""
      }
    };
  },
  created() {},
  methods: {
    handleOpen() {
      this.dialogVisible = true;
    },
    handleClose() {
      this.dialogVisible = false;
    },
    setForm(form){
      this.form=form
    },
    handleClearForm(){
      this.form={}
    },
    submit(){
      if(this.form.id){
        this.$emit('editStudent',{...this.form})
      }else{
      this.$emit('addStudent',{...this.form})
      }
      this.form={}//清零
      this.dialogVisible = false
    }
  },
};
</script>