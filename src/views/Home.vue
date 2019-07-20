<template>
    <div>
      <table-create :currentPage='currentPage' @changePage='changePage' @defaultValue='defaultValue' :cloneTableDataList='cloneTableDataList' :totalCount="totalCount" :tableTitle='tableTitle' :tableDataList='tableDataList'></table-create>
    </div>
</template>
<script>
import TableCreate from '../components/TableCreate' 
export default {
  data(){
    return{
      tableTitle:
      [
        {
          key:'xuehao',
          value:'编号',
          isSort:true
        },
        {
          key:'name',
          value:'名字',
          isSort:false
        },
        {
          key:'type',
          value:'类型',
          isSort:false
        },
        {
          key:'time',
          value:'时间',
          isSort:true
        },
      ], //表格标题
      tableData:[
        {xuehao:1,name:'张三',type:'类型一',time:'2015-01-02'},
        {xuehao:3,name:'往往',type:'类型三',time:'2015-01-04'},
        {xuehao:2,name:'里斯',type:'类型二',time:'2015-01-03'},
        {xuehao:4,name:'里斯',type:'类型二',time:'2015-01-03'},
        {xuehao:5,name:'里斯',type:'类型二',time:'2015-01-03'},
        {xuehao:6,name:'里斯',type:'类型二',time:'2015-01-13'},
        {xuehao:7,name:'里斯',type:'类型二',time:'2015-01-03'},
        {xuehao:8,name:'里斯',type:'类型二',time:'2015-01-03'},
        {xuehao:9,name:'里斯',type:'类型二',time:'2015-01-03'},
        {xuehao:10,name:'里斯',type:'类型二',time:'2015-01-03'},
        {xuehao:11,name:'里斯',type:'类型二',time:'2015-01-03'},
        {xuehao:12,name:'里斯',type:'类型二',time:'2015-01-03'},
        {xuehao:13,name:'里斯',type:'类型二',time:'2015-01-03'},
      ],//表格数据
      totalCount:10, //总页数
      currentPage:1,//当前页
      limit:5,//每页显示的条数
      tableDataList:[],//每页显示的数据
      cloneTableDataList:[]
    }
  },
  created(){
    let count = this.tableData.length / this.limit
    // 总页数
    let digits = String(count).indexOf(".") + 1;//获取小数点的位置
    if(digits > 0){
      this.totalCount = Math.ceil(count)
    }else{
      this.totalCount = count
    }
    this.getList()
  },
  methods:{
    changePage(val){
      // 显示每页对应的数据
      this.currentPage = val
      this.getList()
    },
    defaultValue(){
      this.tableDataList = this.cloneTableDataList
    },
    // 分页过滤
    getList() {
      //过滤分页
      this.tableDataList = this.tableData.filter((item, index) =>
          index < this.currentPage * this.limit && index >= this.limit * (this.currentPage - 1)
      )
      // 简单实现数据深拷贝
       this.cloneTableDataList = JSON.parse(JSON.stringify(this.tableDataList))
    },
  },
  components:{
    TableCreate
  }
}
</script>
<style>

</style>

