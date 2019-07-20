<template>
    <div class="tableCreate">
        <!-- 表格 -->
      <table>
        <thead>
          <tr>
            <th v-for="(item,index) in tableTitle"  @click="sortClick(item.isSort,item.key)" :key="index">
              {{item.value}}
              <span v-if="item.isSort" class="warp">
                <i class="sortCaret down" @click='handleClick($event,item.key, 1)'></i><i class="sortCaret up" @click='handleClick($event,item.key,2)'></i>
              </span>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item,index) in tableDataList" :key="index">
            <td v-for="(itemSub,index) in tableTitle" :key="index">
              {{item[itemSub.key]}}
            </td>
          </tr>
        </tbody>
      </table>
      <!-- 分页 -->
      <ul class="page">
          <li v-for="(item,index) in totalCount" :class="currentPage==item?'activeIndex':''" @click="pageClick(item)" :key="index"> 
              {{item}}
          </li>
      </ul>
    </div>
</template>
<script>
export default {
  name: "TableCreate",
  props: [
    "tableTitle",
    "currentPage",
    "cloneTableDataList",
    "tableDataList",
    "totalCount"
  ],
  data() {
    return {
      sortOrder: 0, //0 默认排序 1 升序排序 2 降序排序
      sortKey: ""
    };
  },
  created() {},
  methods: {
    pageClick(val) {
      //更改每页的数据
      this.$emit("changePage", val);
    },
    sortClick(val, key) {
      if (val) {
        // 保存key

        if (typeof this.sortKey == "undefind") {
          this.sortKey = key;
          this.sortOrder = 0;
        } else if (this.sortKey != key) {
          this.sortKey = key;
          this.sortOrder = 0;
        }
        //与之前的key进行对比
        if (this.sortKey == key) {
          this.sortOrder++;
          if (this.sortOrder > 2) {
            this.sortOrder = 0;
          }
        }
        // 升序还是降序还是保留之前默认的数据
        if (this.sortOrder == 0) {
          this.$emit("defaultValue");
        } else {
          this.sortByKey(this.tableDataList, key);
        }
        //当前下面的颜色改变
      }
    },
    // 排序
    sortByKey(array, key) {
      return array.sort((a, b) => {
        var x = a[key];
        var y = b[key];
        let small = x < y;
        let big = x > y;
        if (this.sortOrder == 1) {
          return small ? -1 : big ? 1 : 0;
        }
        return big ? -1 : small ? 1 : 0;
      });
    },
    // 公用函数
    handleClick(event, key, desc) {
      // desc: 1 升序 2 降序
      // 阻止冒泡
      event.stopPropagation();
      this.sortKey = key;
      this.sortOrder = desc;
      this.sortByKey(this.tableDataList, key);
    }
  }
};
</script>
<style lang="scss">
.tableCreate {
  .warp {
    display: inline-flex;
    flex-direction: column;
    align-items: center;
    height: 24px;
    width: 24px;
    vertical-align: middle;
    cursor: pointer;
    overflow: initial;
    position: relative;
  }
  .sortCaret {
    width: 0;
    height: 0;
    border: 5px solid transparent;
    position: absolute;
    left: 7px;
  }
  .down {
    border-bottom-color: #c0c4cc;
    top: 0px;
  }
  .up {
    border-top-color: #c0c4cc;
    bottom: 2px;
  }
  .activeIndex {
    background: #607d8b;
    color: #fff;
  }
  table {
    border-collapse: collapse;
    tbody {
      tr {
        &:hover {
          cursor: pointer;
          background: #f3f8fb;
          color: #409eff;
        }
      }
    }
    tr {
      th,
      td {
        border: 1px solid #ccc;
        text-align: center;
        &:hover {
          cursor: pointer;
          background: #f3f8fb;
          color: #409eff;
        }
      }

      th {
        padding: 6px 10px;
        min-width: 100px;
      }
    }
  }
  .page {
    li {
      display: inline-block;
      border: 1px solid #ccc;
      padding: 4px 6px;
      list-style-type: none;
      margin-right: 4px;
      cursor: pointer;
    }
    li:hover {
      color: #409eff;
    }
  }
}
</style>


