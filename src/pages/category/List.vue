<template>
     <div>
         
            <el-button type="success" size="small" @click="toAddHandle">添加</el-button>
            <el-button type="danger" size="small">批量删除</el-button>

            <el-table :data="column">
                <el-table-column prop="id" label="编号"></el-table-column>
                <el-table-column prop="name" label="栏目名称"></el-table-column>
                <el-table-column prop="icon" label="图标"></el-table-column>
                <el-table-column prop="num" label="数字"></el-table-column>
                <el-table-column prop="parentId" label="所属"></el-table-column>
                <el-table-column label="操作">
                     <template v-slot="slot">
                     <a href="" @click.prevent="toDeleteHandle()">删除</a>
                     <a href="" @click.prevent="toUpdateHandle()">修改</a>
                     </template>
                </el-table-column>
                </el-table>   
                <!-- 分页 -->
            <el-pagination layout="prev, pager, next" :total="50">
            </el-pagination>
            <!-- 模态框 -->
             <el-dialog
        :title.sync="title" 
        :visible.sync="visible" width="60%">
          {{form}}
          
          <el-form :model="form" label-width="80px">
               <el-form-item label="产品编号">
                    <el-input v-model="form.id 

"></el-input>
               </el-form-item>
                <el-form-item label="栏目名称">
                    <el-input v-model="form.name 

"></el-input>
               </el-form-item>
                <el-form-item label="图标">
                    <el-input v-model="form.icon"></el-input>
               </el-form-item>
                <el-form-item label="数字">
                    <el-input v-model="form.num"></el-input>
               </el-form-item>
               <el-form-item label="所属">
                    <el-input v-model="form.parentId"></el-input>
               </el-form-item>
        </el-form>
  <span slot="footer" class="dialog-footer">
    <el-button size="small" @click="closeModalHandler">取 消</el-button>
    <el-button size="small" type="primary" @click="submitHandler">确 定</el-button>
  </span>
</el-dialog>
     </div>
</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
     created(){
          this.loadData();
     },
     data(){
          return {
               visible:false,
               title:"录入栏目信息",
               column:[],
               form:{}
          }
     },
     methods:{
          loadData(){
               let url="http://localhost:6677/category/findAll"
               request.get(url).then((response)=>{
                    this.column=response.data
               })
          },
          toDeleteHandle(){
                 this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        })
          },
          toUpdateHandle(){
               this.title="修改员工信息";
               this.visible=true;
          },
          closeModalHandler(){
               this.visible=false;
          },
          submitHandler(){
                        let url="http://localhost:6677/category/saveOrUpdate"
            request({
                url,
                method:"POST",
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form)}).then((response)=>{
            //模态框关闭
            this.loadData();
            this.closeModalHandler();
            //提示消息
            this.$message({
              type:"success",
              message:response.message
            })
          })
            
          },
          toAddHandle(){
               this.visible=true;
          }
     }
    
}
</script>
<style scoped>

</style>