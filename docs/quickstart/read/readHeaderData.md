---
title: 读取表头数据
sidebar_position: 6
description: 快速使用easyexcel的来完成excel的读取
---

## 读取表头数据

### excel示例

参照：[最简单的读的excel示例](easyRead#最简单的读的excel示例)

### 对象

参照：[最简单的读的对象](easyRead#最简单的读的对象)

### 监听器

参照：[最简单的读的监听器](easyRead#最简单的读的监听器)
里面多了一个方法,只要重写invokeHeadMap方法即可

```java
    /**
     * 这里会一行行的返回头
     *
     * @param headMap
     * @param context
     */
    @Override
    public void invokeHead(Map<Integer, ReadCellData<?>> headMap, AnalysisContext context) {
        log.info("解析到一条头数据:{}", JSON.toJSONString(headMap));
        // 如果想转成成 Map<Integer,String>
        // 方案1： 不要implements ReadListener 而是 extends AnalysisEventListener
        // 方案2： 调用 ConverterUtils.convertToStringMap(headMap, context) 自动会转换
    }
```

### 代码

```java
    /**
     * 读取表头数据
     *
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link DemoData}
     * <p>
     * 2. 由于默认一行行的读取excel，所以需要创建excel一行一行的回调监听器，参照{@link DemoHeadDataListener}
     * <p>
     * 3. 直接读即可
     */
    @Test
    public void headerRead() {
        String fileName = TestFileUtil.getPath() + "demo" + File.separator + "demo.xlsx";
        // 这里 需要指定读用哪个class去读，然后读取第一个sheet
        EasyExcel.read(fileName, DemoData.class, new DemoHeadDataListener()).sheet().doRead();
    }
```