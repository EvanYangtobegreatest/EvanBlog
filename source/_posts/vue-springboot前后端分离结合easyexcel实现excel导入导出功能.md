---
title: vue+springboot前后端分离结合easyexcel实现excel导入导出功能
date: 2022-09-05 14:59:48
tags:
---

## 使用easyexcel实现excel导入导出功能

基于springboot，前端使用vue，使用阿里的easyexcel实现了单sheet表导入导出，多sheet表导入导出，单数据库表导入导出，多数据库表导入导出。

**首先需要搭建好spingboot和前端开发项目环境**

然后就可以开始开发excel导入导出功能

**首先需要引入easyexcel的maven依赖**

```xml
<!--在pom.xml文件添加以下依赖  -->
 <!--阿里easyexcel 用于进行Excel处理 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>easyexcel</artifactId>
            <version>2.2.10</version>
        </dependency>
```

**根据自己需要的excel表作为对象，建立一个实体类**

有几个注解是重点

```java
//按照数据库表定义好实体类，为其加上注解 @ExcelProperty(value = "xxx", index = 0) ，其中value值对应的是excel表的表头，index的值为数字，代表该列在excel表里面的位置，数字越小列越靠前
// @ExcelIgnore 使用这个注解可以在生成excel时不显示该字段
// @ColumnWidth(20) 自定义列宽
public class User {
    // 主键id
    @ExcelIgnore // 生成报表时忽略，不生成次字段
    private Integer id;
    @ExcelProperty(value = "姓名", index = 0) // 定义表头名称和位置,0代表第一列
    private String name;
    @ExcelProperty(value = "性别", index = 1)
    private String sex;
    @ExcelProperty(value = "年龄", index = 2)
    private Integer age;
    //@ColumnWidth(20) // 定义列宽,使用自适应列宽则注释，不使用则可靠此注解自定义
    @DateTimeFormat(value = "yyyy-MM-dd")
    @ExcelProperty(value = "出生日期", index = 3)
    private String birthday;
}
```

## 结合前后端

#### 导出

```js
 //vue的执行函数
exportWeb: function () {
              // 使用axios进行GET请求
                   this.$axios.get('/exportExcel', {
                           responseType: 'blob', // 设置响应类型为blob
                         })
                           .then(response => {
                             // 创建一个Blob对象，并下载文件
                             const blob = new Blob([response.data], { type: response.headers['content-type'] });
                             const link = document.createElement('a');
                             link.href = window.URL.createObjectURL(blob);
                             link.download = '导出'; // 设置下载的文件名
                             link.click();
                           })
                           .catch(error => {
                             // 处理请求错误
                             console.error('文件下载失败:', error);
                           });
           },
```

```java
  @RequestMapping("/api/exportExcel")
    public void export(HttpServletResponse response) throws IOException {

        response.setContentType("application/vnd.openxmlformats-officedocument.spreadsheetml.sheet");
//        response.setCharacterEncoding("utf-8");
//       String fileName = URLEncoder.encode("导出文件名", "UTF-8").replaceAll("\\+", "%20");
//      response.setHeader("Content-disposition", "attachment;filename*=utf-8''" + fileName + ".xlsx");

        //查询数据
        List<Student> student = studentService.queryStudent();
        EasyExcel.write(response.getOutputStream(), Student.class).sheet("导出文件名").doWrite(student);
    }
```

#### 导入

```html
 //导出页面
 <template>

    <el-upload
            class="upload-demo"
            ref="upload"
            action="http://localhost:8082/api/upload"
            :on-preview="handlePreview"
            :on-remove="handleRemove"
            :on-success="handleSuccess"
            :file-list="fileList"
            :multiple="false"
            :limit="1"
            :auto-upload="false">
        <el-button slot="trigger" size="small" type="primary">选取文件</el-button>
        <el-button style="margin-left: 10px;" size="small" type="success" @click="submitUpload($event)">上传到服务器</el-button>
        <div slot="tip" class="el-upload__tip">只能上传execl文件，且不超过500kb</div>
    </el-upload>

</template>

<script>
    export default {
        data() {
            return {
                limitNum: 1,  // 上传excel时，同时允许上传的最大数
                fileList: [],   // excel文件列表
            }
        },
        methods:{
            // 成功上传到服务器后返回的内容
            handleSuccess(e) {
                console.log('上传成功', e)
                this.$message(e.message);
            },
            // 上传文件到服务器
            submitUpload(e) {

                console.log('我要上传了', e)
                this.$refs.upload.submit();
            },

            // 删除文件
            handleRemove(file, fileList) {
                console.log(file, fileList);
            },
            handlePreview(file) {
                console.log(file);
            },
        }
    }
</script>

<style scoped>

</style>
```

```java
// 导出接口
// easyexcel上传文件
    @PostMapping("/api/upload")
    @ResponseBody
    public String upload(MultipartFile file) throws IOException {

        EasyExcel.read(file.getInputStream(), Student.class, new StudentDataListener(studentService)).sheet().doRead();
        return "上传成功";
    }
```

##### 可以用配置类配置文件上传的大小

```java
@Configuration
public class MulterFile {
    /**
     * 文件上传配置
     * @return
     */
    @Bean
    public MultipartConfigElement multipartConfigElement() {
        MultipartConfigFactory factory = new MultipartConfigFactory();
        //文件最大
        factory.setMaxFileSize(DataSize.parse("30960KB")); //KB,MB
        /// 设置总上传数据总大小
        factory.setMaxRequestSize(DataSize.parse("309600KB"));
        return factory.createMultipartConfig();
    }
}
```

