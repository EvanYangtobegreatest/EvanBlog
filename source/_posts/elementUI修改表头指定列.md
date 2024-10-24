---
title: elementUI修改表头指定列
date: 2022-05-13 10:22:33
author: Evan
categories: 笔记
index_img: /img/bg9.jpg
tags:
- elementUI

---




## elementUI如何修改表头的某一列样式？

今天碰到个需求，要在elementUI 的表格头上根据不同的列名称改变颜色。

如图所示：

![](/img/elementui-%E7%AC%94%E8%AE%B0/headerRowStyle.PNG)

### 实现方法

```vue
//调用headerRowStyle方法
<el-table :data="tableData" border :header-cell-style="headerRowStyle">
```

定义headerRowStyle方法

```js
 headerRowStyle(obj){
     //根据列的标签名来指定修改
        if(obj.column.label=="日期"){ 
          return 'color: #fff;background:#00bfbf'
        }
        if(obj.column.label=="姓名"){
          return 'color: #fff;background:#ffd454'
        }
        if(obj.column.label=="地址") {
          return 'color:#00ffff'
        }
      }
```

完整代码如下：

```vue
<template>
  <el-table
    :data="tableData"
    border
    style="width: 100%" :header-cell-style="headerRowStyle">
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
          date: '2016-05-04',
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
        }]
      }
    },
    created() {
      this.setdates(this.tableData)
    },
    methods: {


      headerRowStyle(obj){


        if(obj.column.label=="日期"){
          return 'color: #fff;background:#00bfbf'
        }
        if(obj.column.label=="姓名"){
          return 'color: #fff;background:#ffd454'
        }
        if(obj.column.label=="地址") {
          return 'color:#00ffff'
        }
      },
    }
  };
</script>



<style scoped>

</style>



```

