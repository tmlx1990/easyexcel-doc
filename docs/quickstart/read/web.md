---
title: web中的读
sidebar_position: 11
description: 快速使用easyexcel的来完成excel的读取
---


## web中的读

### 示例代码

DEMO代码地址：[https://github.com/alibaba/easyexcel/blob/master/easyexcel-test/src/test/java/com/alibaba/easyexcel/test/demo/web/WebTest.java](https://github.com/alibaba/easyexcel/blob/master/easyexcel-test/src/test/java/com/alibaba/easyexcel/test/demo/web/WebTest.java)

### excel示例

参照：[最简单的读的excel示例](easyRead#最简单的读的excel示例)

### 对象

参照：[最简单的读的对象](easyRead#最简单的读的对象) 只是名字变了

### 监听器

参照：[最简单的读的监听器](easyRead#最简单的读的监听器) 只是泛型变了

### 代码

```java
    /**
     * 文件上传
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link UploadData}
     * <p>
     * 2. 由于默认一行行的读取excel，所以需要创建excel一行一行的回调监听器，参照{@link UploadDataListener}
     * <p>
     * 3. 直接读即可
     */
    @PostMapping("upload")
    @ResponseBody
    public String upload(MultipartFile file) throws IOException {
        EasyExcel.read(file.getInputStream(), UploadData.class, new UploadDataListener(uploadDAO)).sheet().doRead();
        return "success";
    }
```
