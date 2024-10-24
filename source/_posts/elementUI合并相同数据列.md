---
title: elementUI合并相同数据列
date: 2022-05-13 16:42:19
author: Evan
categories: 笔记
tags:
- elementUI
---



## elementUI 表格 如何合并相同数据的列？

在编写表格table中，会出现合并数据相同的列这种需求，如图：

![](/img/elementui-%E7%AC%94%E8%AE%B0/objectSpanMethod.PNG)

### 实现方法

添加span-method 方法，elementUI提供了span-method方法 让我们可以动态合并表格，其中有四个参数，分别是row, column, rowIndex, columnIndex

```vue
<!--   
row:代表当前行的值
column:代表当前列的值
rowIndex:当前行的索引
columnIndex:当前列的索引
-->

<el-table :data="tableData" :span-method="objectSpanMethod" >

<!--objectSpanMethod是定义的逻辑方法，可以根据自己需求定义-->
```

objectSpanMethod的具体实现

```js
setdates(arr) {   //获取数组
        var obj = {},
          k, arr1 = [];
        for(var i = 0, len = arr.length; i < len; i++) {
          k = arr[i].date;//合并所需要的列
          if(obj[k])
            obj[k]++;
          else
            obj[k] = 1;
        }
        console.log(obj)
        //保存结果{el-'元素'，count-出现次数}
        for(var o in obj) {
          for(let i=0;i<obj[o];i++){
            if(i===0){
              this.arr1.push(obj[o])
            }else{
              this.arr1.push(0)
            }
          }
        }
        console.log(this.arr1);

      },
      objectSpanMethod({ row, column, rowIndex, columnIndex }) {
        if (columnIndex === 0 ) {
          let _row = this.arr1[rowIndex]
          let _col = this.arr1[rowIndex] > 0 ? 1 : 0
          return [_row,_col]
        }
      }
```



## 完整代码

```vue
<template>
  <el-table
    :data="tableData"
    border
    style="width: 100%"  :span-method="objectSpanMethod" >
    <el-table-column
      prop="date"
      label="日期"
      width="180">
    </el-table-column>
    <el-table-column
      prop="name"
      label="姓名"
      width="180">
    </el-table-column>
    <el-table-column
      prop="address"
      label="地址">
    </el-table-column>
  </el-table>
</template>

<script>
  export default {
    name: "table",
    data() {
      return {
        tableData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }],
        arr1:[],
        arr2:[]
      }
    },
    created() {
      this.setdates(this.tableData)
    },
    methods: {
      setdates(arr) {
        var obj = {},
          k, arr1 = [];
        for(var i = 0, len = arr.length; i < len; i++) {
          k = arr[i].date;//需要合并的字段
          if(obj[k])
            obj[k]++;
          else
            obj[k] = 1;
        }
        console.log(obj)
        //保存结果{el-'元素'，count-出现次数}
        for(var o in obj) {
          for(let i=0;i<obj[o];i++){
            if(i===0){
              this.arr1.push(obj[o])
            }else{
              this.arr1.push(0)
            }
          }
        }
        console.log(this.arr1);

      },


      objectSpanMethod({ row, column, rowIndex, columnIndex }) {
        if (columnIndex === 0 ) {
          let _row = this.arr1[rowIndex]
          let _col = this.arr1[rowIndex] > 0 ? 1 : 0
          return [_row,_col]
        }
      }
    }
  };
</script>



<style scoped>

</style>

```

