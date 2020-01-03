<template>
    <div>
            <el-button size="small" type="success" @click="toAddHandler">添加</el-button>
            <el-button size="small" type="danger">批量删除</el-button>

            <el-table :data="product">
                <el-table-column prop="id" label="编号"></el-table-column>
                <el-table-column prop="name" label="产品名称"></el-table-column>
                <el-table-column prop="price" label="价格"></el-table-column>
                <el-table-column prop="description" label="描述"></el-table-column>
                <el-table-column prop="categoryId" label="所属产品"></el-table-column>
                <el-table-column label="操作">
                    <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandle(slot.row.id 

)">删除</a>
                      <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>
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
              
              <el-form-item label="产品名称">
                  <el-input v-model="form.name 

"></el-input> 
              </el-form-item>
              <el-form-item label="描述">
                  <el-input v-model="form.description"></el-input>
              </el-form-item>
              <el-form-item label="价格">
                  <el-input v-model="form.price"></el-input>
              </el-form-item>
              <el-form-item label="所属产品">
                  <el-input v-model="form.categoryId"></el-input>
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
            title:"录入产品信息",
            visible:false,
            product:[],
            form:{
          type:"product"
            }
        }

    },
    methods:{
       
        loadData(){
            let url="http://localhost:6677/product/findAll"
            request.get(url).then((response)=>{
                this.product=response.data
            })
        },
        closeModalHandler(){
          this.visible=false;
        },
        submitHandler(){
                     let url="http://localhost:6677/product/saveOrUpdate"
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
          this.visible=true;
        },
        //添加
        toAddHandler(){
            this.visible=true;
            this.title="录入员工信息";

        }
    }
    
}
</script>
<style scoped>

</style>
