<template>
    <div>
      <!-- @为绑定方法 -->
      <el-button type = "success" size="small" @click="toAddHandler">添加</el-button>
      <el-button type = "danger" size="small">批量删除</el-button>
        <!-- data为对应方法 -->
     <el-table :data="customers">
        <!-- prop显示对应内容 -->
     <el-table-column prop="id" label="编号"></el-table-column>
     <el-table-column prop="realname"  label="姓名"></el-table-column>
     <el-table-column prop="gender"  label="性别"></el-table-column>
     <el-table-column prop="telephone"    label="联系方式"></el-table-column>
     <el-table-column   label="操作">
         <!-- 跟踪位置  卡槽  接收每一行数据 -->
         <template v-slot="slot">
             <!-- 阻止跳转到默认页面    id为删除方法的索引  获取当前行  {{}}打印-->
           <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
           <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>   
         </template>         
    </el-table-column>    
    </el-table>
    <!-- 表格结束 -->
    <!-- 分页开始 -->
    <!-- <el-pagination
    layout="prev, pager, next"
    :total="50">
   </el-pagination> -->

   <!-- 模态框 -->
   <el-dialog title="录入顾客信息"
   :visible.sync="visible"
    width="60%">
    <!-- //为啥这写form -->
    <!-- {{form}} -->
    <el-form :model="form" label-width="80px">
      <el-form-item label="用户名">
        <el-input v-model="form.username"></el-input>
      </el-form-item>
    
      <el-form-item label="密码">
        <el-input  type ="password" v-model="form.password"></el-input>
      </el-form-item>

      <el-form-item label="真实姓名">
        <el-input  type ="realname" v-model="form.realname"></el-input>
      </el-form-item>

      <el-form-item label="电话号码">
        <el-input  type ="telephone" v-model="form.telephone"></el-input>
      </el-form-item>

    </el-form>
    <span slot="footer" class="dialog-footer">
    <el-button size="small"  @click="closeModalHandler">取 消</el-button>
    <el-button size="small" type="primary" @click="submitHandle">确 定</el-button>
  </span>
  </el-dialog>
    </div>
</template>
// 模板
<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    //用于存放网页中需要调用的方法
    methods:{
      //方法封装，为了减少代码chenyu
      loadData(){
      let url = "http://localhost:6677//customer/findAll"
      request.get(url).then((response)=>{
        //将查询结果设置到customers中,this指向外部函数的this
        this.customers = response.data;
      })
      },

        submitHandle(){
            //对表单中的数据进行储存
            //this.form 对象---字符串--- 后台（type:'customer',age:12）
            //json'{"type":"customer","age":12}

            //通过request与后台进行交流，并且要携带参数
            let url="http://localhost:6677/customer/saveOrUpdate"
            request({
                url,
                method:"POST",
                headers:{
                    "Content-Type":"application/x-www-form-urlencoded"
                },
                data:querystring.stringify(this.form)
            }).then((response)=>{
                //请求结束
                this.closeModalHandle();
                this.loadData();
                this.$message({
                    type:"success",
                    message:response.message
                })
            })

        },
        toDeleteHandler(id){
            //确认
            //alert(id);
          this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          //调用后台接口完成删除操作
          let url = "http://localhost:6677/customer/deleteById?id"+id;
          request.get(url).then((response)=>{
            //刷新数据
            this.loadData();
            //提示结果
            this.$message({
            type: 'success',
            message: responsse.message
          });

          });
        })
        },
        toUpdateHandler(row){
          //显示当前行的信息
           this.form = row;
           this.visible=true;
        },

        closeModalHandle(){
           this.visible=false; 
        },

        toAddHandler(){
          this.form = {
            //初始信息
            type:"customer"
          }
            this.visible=true;
        }
    },
    //用于存放要向网页中显示的数据
    data(){
        return{
            visible:false,
            customers:[],
            form:{
              type:"customer"
            }
        }
    },
    created(){
      //vue实例创建完毕
      this.loadData();

    }
}
</script>
//脚本
<style scoped>

</style>
//样式