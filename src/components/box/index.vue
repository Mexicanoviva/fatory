<template>
<div class="page-container">
<div class="search-wrapper">

<div class="block">
<div class="label">订单号</div>
<el-input v-model="searchForm.orderId"></el-input>
</div>
<div class="block">
<div class="label">箱号</div>
<el-input v-model="searchForm.boxNum"></el-input>
</div>
 <div class="operates">
  <el-button type="primary" @click="searchClick">查询</el-button>
  <el-button type="primary" @click="resetClick">重置</el-button>
</div>
</div>
<el-table
      :data="tableData"
      style="width:100%">
      <el-table-column
        prop="OrderID"
        label="订单号"
        >
      </el-table-column>
      <el-table-column
        prop="Box"
        label="箱号"
        >
      </el-table-column>
      <el-table-column
        prop="SnStart"
        label="SN号起始">
      </el-table-column>
      <el-table-column
        prop="SnEnd"
        label="SN号终止"
       >
      </el-table-column>
    
     
    </el-table>
              <el-pagination
               @current-change="pageChange"
      :current-page.sync="pageData.Page"
      :page-size="pageData.RowNum"
      layout="total, prev, pager, next"
      :total="RowNum">
    </el-pagination>
      
</div>
</template>
<script>
import {BOX_API_PATH} from "../../service/api"

export default{
    data(){
        return{
            searchForm:{
                orderId:"",
                boxNum:""
            },
            tableData:[],
            RowNum:0,
            pageData:{
                Page:1,
                RowNum:20
            }
        }
    },
    created(){
      
       this.searchForm.orderId = this.$route.query.orderId?this.$route.query.orderId:""
        this.getBoxList()
    },
    methods:{
        getBoxList(){
            let formData = new FormData()
        formData.append("Token",sessionStorage.getItem("token"))
        formData.append("Act","GetBoxList")
        formData.append("Page",this.pageData.Page-1)
        formData.append("RowNum",this.pageData.RowNum)
        formData.append("Box",this.searchForm.boxNum)
        formData.append("OrderID",this.searchForm.orderId)
   
       
        this.$axios.post(BOX_API_PATH,formData).then(res=>{
          if(res.data.Ret == 0){
            this.tableData = res.data.Data?res.data.Data:[]
            this.RowNum = res.data.Recordcount
          }else{
			  this.$message({
			  	message:res.data.Msg,
			  	type:"error",
			  	duration:3000
			  });
		  }
        })
        },
        resetClick(){
            this.searchForm = {
                orderId:"",
                boxNum:""
            }
        },
        searchClick(){
          this.getBoxList()
        },
        pageChange(){
          this.getBoxList()
        }
    }
}
</script>
<style scoped>
.search-wrapper{
display:flex;
justy-content:flex-start;
flex-wrap:wrap;
}


</style>