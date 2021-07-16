<!--<template>-->
<!--  <div class="trend-Area">-->
<!--    <el-table-->
<!--        :data="tableData.slice((currPage-1)*pageSize,currPage*pageSize)"//前端分页的重点在这-->
<!--    stripe-->
<!--    style="width: 100%">-->
<!--    <el-table-column-->
<!--        prop="taxonomy"-->
<!--        label="Taxonomy" >-->
<!--    </el-table-column>-->
<!--    </el-table>-->
<!--    <div style="width:100%;display:flex;justify-content:space-between;">-->
<!--      <div class="count-show">-->
<!--             <span>-->
<!--                 total: {{total}} items-->
<!--             </span>-->
<!--      </div>-->
<!--      <el-pagination-->
<!--          :current-page="currPage"-->
<!--          @size-change="handleSizeChange"-->
<!--          @current-change="handleCurrentChange"-->
<!--          :page-size="pageSize"-->
<!--          :page-sizes="[50, 100]"-->
<!--          layout="prev, pager, next, jumper"-->
<!--          :total="total">-->
<!--      </el-pagination>-->
<!--      <div class="custom-pagination">-->
<!--        Results Per Page-->
<!--        <el-select v-model="pageSize" size="small" placeholder="" @change="handleSizeChange">-->
<!--          <el-option-->
<!--              v-for="(item,index) in perpage"-->
<!--              :key="index"-->
<!--              :label="item.label"-->
<!--              :value="item.value">-->
<!--          </el-option>-->
<!--        </el-select>-->
<!--      </div>-->
<!--    </div>-->
<!--  </div>-->
<!--</template>-->

<!--<script>-->
<!--export default {-->
<!--  data() {-->
<!--    return{-->
<!--      total:0,-->
<!--      currPage: 1,  //默认第一页-->
<!--      pageSize: 50, //默认展示50条数据-->
<!--      perpage: [-->
<!--        {value: 50, label: "50"},-->
<!--        {value: 100, label: "100"},-->
<!--      ],-->
<!--    }-->
<!--  },-->
<!--  methods: {-->
<!--    getData(){-->
<!--      //请求接口后取数据给tableData赋值-->
<!--      //请求的接口的参数给个参考：-->
<!--      /**-->
<!--       data:{-->
<!--        			 keywords:this.input  //双向绑定的关键字-->
<!--                 },-->
<!--       返回res.data=this.tableData-->
<!--       **/-->
<!--    },-->
<!--    handleSizeChange(val) {-->
<!--      //重置当前页为第一页-->
<!--      this.pageSize = val;-->
<!--      this.currPage = 1;-->
<!--    },-->
<!--    handleCurrentChange(val) {-->
<!--      //点击改变当前页-->
<!--      this.currPage = val;-->
<!--    },-->
<!--  }-->
<!--}-->


<!--</script>-->
<!--<style scoped>-->
<!--*{-->
<!--  user-select: none;-->
<!--  /* 无法选中，整体感更强 */-->
<!--}-->
<!--body{-->
<!--  background: url("../../assets/wallpaper.jpg") no-repeat fixed;-->
<!--}-->
<!--#yui{-->
<!--  margin-left: -470px;-->
<!--  width: min-content;-->
<!--  height: min-content;-->
<!--}-->
<!--.ti{-->
<!--  margin-left: -160px;-->
<!--  margin-top: 8px;-->
<!--  width: max-content;-->
<!--  height: max-content;-->
<!--}-->
<!--.login{-->
<!--  position: absolute;-->
<!--  top: 50%;-->
<!--  margin-top: -281px;-->
<!--  left: 50%;-->
<!--  margin-left: -200px;-->
<!--  /* absolute居中的一种方法 */-->
<!--  background-color: whitesmoke;-->
<!--  width: 400px;-->
<!--  height: 557px;-->
<!--  border-radius: 25px;-->
<!--  text-align: center;-->
<!--  padding: 5px 40px;-->
<!--  box-sizing: border-box;-->
<!--  /* 这样padding就不会影响大小 */-->
<!--}-->
<!--p{-->
<!--  font-size: 42px;-->
<!--  font-weight: 600;-->
<!--}-->
<!--input{-->
<!--  background-color: whitesmoke;-->
<!--  width: 100%;-->
<!--  height: 40px;-->
<!--  margin-bottom: 10px;-->
<!--  border: none;-->
<!--  border-bottom: 2px solid silver;-->
<!--  /* 下面的会覆盖上面的步伐 */-->
<!--  outline: none;-->
<!--  font-size: 22px;-->
<!--}-->

<!--.btn{-->
<!--  display: block;-->
<!--  background-color:#3399CC ;-->
<!--  width: 42%;-->
<!--  height: 52px;-->
<!--  border-radius: 23px;-->
<!--  margin-top: 37px;-->
<!--  font-size: 31px;-->
<!--  font-weight: 600;-->
<!--  color: white;-->
<!--  float:left;-->
<!--}-->
<!--.btn1-->
<!--{-->
<!--  margin-left: 40px;-->
<!--}-->
<!--.btn:hover{-->
<!--  background-color:#17D3AD-->
<!--}-->

<!--</style>-->
