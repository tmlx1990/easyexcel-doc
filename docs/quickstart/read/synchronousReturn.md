---
title: 同步的返回
sidebar_position: 5
description: 快速使用easyexcel的来完成excel的读取
---

## 同步的返回

### excel示例

参照：[最简单的读的excel示例](easyRead.md#最简单的读的excel示例)

### 对象

参照：[最简单的读的对象](easyRead.md#最简单的读的对象)

### 代码

```java
    /**
     * 同步的返回，不推荐使用，如果数据量大会把数据放到内存里面
     */
    @Test
    public void synchronousRead() {
        String fileName = TestFileUtil.getPath() + "demo" + File.separator + "demo.xlsx";
        // 这里 需要指定读用哪个class去读，然后读取第一个sheet 同步读取会自动finish
        List<DemoData> list = EasyExcel.read(fileName).head(DemoData.class).sheet().doReadSync();
        for (DemoData data : list) {
            LOGGER.info("读取到数据:{}", JSON.toJSONString(data));
        }
        // 这里 也可以不指定class，返回一个list，然后读取第一个sheet 同步读取会自动finish
        List<Map<Integer, String>> listMap = EasyExcel.read(fileName).sheet().doReadSync();
        for (Map<Integer, String> data : listMap) {
            // 返回每条数据的键值对 表示所在的列 和所在列的值
            LOGGER.info("读取到数据:{}", JSON.toJSONString(data));
        }
    }
```
