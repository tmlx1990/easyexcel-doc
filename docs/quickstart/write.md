---
title: 写Excel
sidebar_position: 2
description: 快速使用easyexcel的来完成excel的写入
---




## 列宽、行高

### excel示例

![img](../img/quickstart/write/widthAndHeightWrite.png)

### 对象

```java
@Getter
@Setter
@EqualsAndHashCode
@ContentRowHeight(10)
@HeadRowHeight(20)
@ColumnWidth(25)
public class WidthAndHeightData {
    @ExcelProperty("字符串标题")
    private String string;
    @ExcelProperty("日期标题")
    private Date date;
    /**
     * 宽度为50
     */
    @ColumnWidth(50)
    @ExcelProperty("数字标题")
    private Double doubleData;
}
```

### 代码

```java
    /**
     * 列宽、行高
     * <p>1. 创建excel对应的实体对象 参照{@link WidthAndHeightData}
     * <p>2. 使用注解{@link ColumnWidth}、{@link HeadRowHeight}、{@link ContentRowHeight}指定宽度或高度
     * <p>3. 直接写即可
     */
    @Test
    public void widthAndHeightWrite() {
        String fileName = TestFileUtil.getPath() + "widthAndHeightWrite" + System.currentTimeMillis() + ".xlsx";
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        EasyExcel.write(fileName, WidthAndHeightData.class).sheet("模板").doWrite(data());
    }
```

## 注解形式自定义样式

### since

2.2.0-beta1

### excel示例

![img](../img/quickstart/write/annotationStyleWrite.png)

### 对象

```java
@Getter
@Setter
@EqualsAndHashCode
// 头背景设置成红色 IndexedColors.RED.getIndex()
@HeadStyle(fillPatternType = FillPatternTypeEnum.SOLID_FOREGROUND, fillForegroundColor = 10)
// 头字体设置成20
@HeadFontStyle(fontHeightInPoints = 20)
// 内容的背景设置成绿色 IndexedColors.GREEN.getIndex()
@ContentStyle(fillPatternType = FillPatternTypeEnum.SOLID_FOREGROUND, fillForegroundColor = 17)
// 内容字体设置成20
@ContentFontStyle(fontHeightInPoints = 20)
public class DemoStyleData {
    // 字符串的头背景设置成粉红 IndexedColors.PINK.getIndex()
    @HeadStyle(fillPatternType = FillPatternTypeEnum.SOLID_FOREGROUND, fillForegroundColor = 14)
    // 字符串的头字体设置成20
    @HeadFontStyle(fontHeightInPoints = 30)
    // 字符串的内容的背景设置成天蓝 IndexedColors.SKY_BLUE.getIndex()
    @ContentStyle(fillPatternType = FillPatternTypeEnum.SOLID_FOREGROUND, fillForegroundColor = 40)
    // 字符串的内容字体设置成20
    @ContentFontStyle(fontHeightInPoints = 30)
    @ExcelProperty("字符串标题")
    private String string;
    @ExcelProperty("日期标题")
    private Date date;
    @ExcelProperty("数字标题")
    private Double doubleData;
}/**
 * 样式的数据类
 *
 * @author Jiaju Zhuang
 **/
@Data
// 头背景设置成红色 IndexedColors.RED.getIndex()
@HeadStyle(fillPatternType = FillPatternType.SOLID_FOREGROUND, fillForegroundColor = 10)
// 头字体设置成20
@HeadFontStyle(fontHeightInPoints = 20)
// 内容的背景设置成绿色 IndexedColors.GREEN.getIndex()
@ContentStyle(fillPatternType = FillPatternType.SOLID_FOREGROUND, fillForegroundColor = 17)
// 内容字体设置成20
@ContentFontStyle(fontHeightInPoints = 20)
public class DemoStyleData {
    // 字符串的头背景设置成粉红 IndexedColors.PINK.getIndex()
    @HeadStyle(fillPatternType = FillPatternType.SOLID_FOREGROUND, fillForegroundColor = 14)
    // 字符串的头字体设置成20
    @HeadFontStyle(fontHeightInPoints = 30)
    // 字符串的内容的背景设置成天蓝 IndexedColors.SKY_BLUE.getIndex()
    @ContentStyle(fillPatternType = FillPatternType.SOLID_FOREGROUND, fillForegroundColor = 40)
    // 字符串的内容字体设置成20
    @ContentFontStyle(fontHeightInPoints = 30)
    @ExcelProperty("字符串标题")
    private String string;
    @ExcelProperty("日期标题")
    private Date date;
    @ExcelProperty("数字标题")
    private Double doubleData;
}
```

### 代码

```java
    /**
     * 注解形式自定义样式
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link DemoStyleData}
     * <p>
     * 3. 直接写即可
     *
     * @since 2.2.0-beta1
     */
    @Test
    public void annotationStyleWrite() {
        String fileName = TestFileUtil.getPath() + "annotationStyleWrite" + System.currentTimeMillis() + ".xlsx";
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        EasyExcel.write(fileName, DemoStyleData.class).sheet("模板").doWrite(data());
    }
```

## 自定义样式

### excel示例

![img](../img/quickstart/write/styleWrite.png)

### 对象

参照：[最简单的写的对象](#最简单的写的对象)

### 代码

```java
    /**
     * 拦截器形式自定义样式
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link DemoData}
     * <p>
     * 2. 创建一个style策略 并注册
     * <p>
     * 3. 直接写即可
     */
    @Test
    public void handlerStyleWrite() {
        // 方法1 使用已有的策略 推荐
        // HorizontalCellStyleStrategy 每一行的样式都一样 或者隔行一样
        // AbstractVerticalCellStyleStrategy 每一列的样式都一样 需要自己回调每一页
        String fileName = TestFileUtil.getPath() + "handlerStyleWrite" + System.currentTimeMillis() + ".xlsx";
        // 头的策略
        WriteCellStyle headWriteCellStyle = new WriteCellStyle();
        // 背景设置为红色
        headWriteCellStyle.setFillForegroundColor(IndexedColors.RED.getIndex());
        WriteFont headWriteFont = new WriteFont();
        headWriteFont.setFontHeightInPoints((short)20);
        headWriteCellStyle.setWriteFont(headWriteFont);
        // 内容的策略
        WriteCellStyle contentWriteCellStyle = new WriteCellStyle();
        // 这里需要指定 FillPatternType 为FillPatternType.SOLID_FOREGROUND 不然无法显示背景颜色.头默认了 FillPatternType所以可以不指定
        contentWriteCellStyle.setFillPatternType(FillPatternType.SOLID_FOREGROUND);
        // 背景绿色
        contentWriteCellStyle.setFillForegroundColor(IndexedColors.GREEN.getIndex());
        WriteFont contentWriteFont = new WriteFont();
        // 字体大小
        contentWriteFont.setFontHeightInPoints((short)20);
        contentWriteCellStyle.setWriteFont(contentWriteFont);
        // 这个策略是 头是头的样式 内容是内容的样式 其他的策略可以自己实现
        HorizontalCellStyleStrategy horizontalCellStyleStrategy =
            new HorizontalCellStyleStrategy(headWriteCellStyle, contentWriteCellStyle);

        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        EasyExcel.write(fileName, DemoData.class)
            .registerWriteHandler(horizontalCellStyleStrategy)
            .sheet("模板")
            .doWrite(data());

        // 方法2: 使用easyexcel的方式完全自己写 不太推荐 尽量使用已有策略
        // @since 3.0.0-beta2
        fileName = TestFileUtil.getPath() + "handlerStyleWrite" + System.currentTimeMillis() + ".xlsx";
        EasyExcel.write(fileName, DemoData.class)
            .registerWriteHandler(new CellWriteHandler() {
                @Override
                public void afterCellDispose(CellWriteHandlerContext context) {
                    // 当前事件会在 数据设置到poi的cell里面才会回调
                    // 判断不是头的情况 如果是fill 的情况 这里会==null 所以用not true
                    if (BooleanUtils.isNotTrue(context.getHead())) {
                        // 第一个单元格
                        // 只要不是头 一定会有数据 当然fill的情况 可能要context.getCellDataList() ,这个需要看模板，因为一个单元格会有多个 WriteCellData
                        WriteCellData<?> cellData = context.getFirstCellData();
                        // 这里需要去cellData 获取样式
                        // 很重要的一个原因是 WriteCellStyle 和 dataFormatData绑定的 简单的说 比如你加了 DateTimeFormat
                        // ，已经将writeCellStyle里面的dataFormatData 改了 如果你自己new了一个WriteCellStyle，可能注解的样式就失效了
                        // 然后 getOrCreateStyle 用于返回一个样式，如果为空，则创建一个后返回
                        WriteCellStyle writeCellStyle = cellData.getOrCreateStyle();
                        writeCellStyle.setFillForegroundColor(IndexedColors.RED.getIndex());
                        // 这里需要指定 FillPatternType 为FillPatternType.SOLID_FOREGROUND
                        writeCellStyle.setFillPatternType(FillPatternType.SOLID_FOREGROUND);

                        // 这样样式就设置好了 后面有个FillStyleCellWriteHandler 默认会将 WriteCellStyle 设置到 cell里面去 所以可以不用管了
                    }
                }
            }).sheet("模板")
            .doWrite(data());

        // 方法3: 使用poi的样式完全自己写 不推荐
        // @since 3.0.0-beta2
        // 坑1：style里面有dataformat 用来格式化数据的 所以自己设置可能导致格式化注解不生效
        // 坑2：不要一直去创建style 记得缓存起来 最多创建6W个就挂了
        fileName = TestFileUtil.getPath() + "handlerStyleWrite" + System.currentTimeMillis() + ".xlsx";
        EasyExcel.write(fileName, DemoData.class)
            .registerWriteHandler(new CellWriteHandler() {
                @Override
                public void afterCellDispose(CellWriteHandlerContext context) {
                    // 当前事件会在 数据设置到poi的cell里面才会回调
                    // 判断不是头的情况 如果是fill 的情况 这里会==null 所以用not true
                    if (BooleanUtils.isNotTrue(context.getHead())) {
                        Cell cell = context.getCell();
                        // 拿到poi的workbook
                        Workbook workbook = context.getWriteWorkbookHolder().getWorkbook();
                        // 这里千万记住 想办法能复用的地方把他缓存起来 一个表格最多创建6W个样式
                        // 不同单元格尽量传同一个 cellStyle
                        CellStyle cellStyle = workbook.createCellStyle();
                        cellStyle.setFillForegroundColor(IndexedColors.RED.getIndex());
                        // 这里需要指定 FillPatternType 为FillPatternType.SOLID_FOREGROUND
                        cellStyle.setFillPattern(FillPatternType.SOLID_FOREGROUND);
                        cell.setCellStyle(cellStyle);

                        // 由于这里没有指定dataformat 最后展示的数据 格式可能会不太正确

                        // 这里要把 WriteCellData的样式清空， 不然后面还有一个拦截器 FillStyleCellWriteHandler 默认会将 WriteCellStyle 设置到
                        // cell里面去 会导致自己设置的不一样
                        context.getFirstCellData().setWriteCellStyle(null);
                    }
                }
            }).sheet("模板")
            .doWrite(data());
    }
```

## 合并单元格

### since

2.2.0-beta1

### excel示例

![img](../img/quickstart/write/mergeWrite.png)

### 对象

方法1

```java
@Getter
@Setter
@EqualsAndHashCode
// 将第6-7行的2-3列合并成一个单元格
// @OnceAbsoluteMerge(firstRowIndex = 5, lastRowIndex = 6, firstColumnIndex = 1, lastColumnIndex = 2)
public class DemoMergeData {
    // 这一列 每隔2行 合并单元格
    @ContentLoopMerge(eachRow = 2)
    @ExcelProperty("字符串标题")
    private String string;
    @ExcelProperty("日期标题")
    private Date date;
    @ExcelProperty("数字标题")
    private Double doubleData;
}
```

方法2参照：[最简单的写的对象](#最简单的写的对象)

### 代码

```java
   /**
     * 合并单元格
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link DemoData} {@link DemoMergeData}
     * <p>
     * 2. 创建一个merge策略 并注册
     * <p>
     * 3. 直接写即可
     *
     * @since 2.2.0-beta1
     */
    @Test
    public void mergeWrite() {
        // 方法1 注解
        String fileName = TestFileUtil.getPath() + "mergeWrite" + System.currentTimeMillis() + ".xlsx";
        // 在DemoStyleData里面加上ContentLoopMerge注解
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        EasyExcel.write(fileName, DemoMergeData.class).sheet("模板").doWrite(data());

        // 方法2 自定义合并单元格策略
        fileName = TestFileUtil.getPath() + "mergeWrite" + System.currentTimeMillis() + ".xlsx";
        // 每隔2行会合并 把eachColumn 设置成 3 也就是我们数据的长度，所以就第一列会合并。当然其他合并策略也可以自己写
        LoopMergeStrategy loopMergeStrategy = new LoopMergeStrategy(2, 0);
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        EasyExcel.write(fileName, DemoData.class).registerWriteHandler(loopMergeStrategy).sheet("模板").doWrite(data());
    }
```

## 使用table去写入

### excel示例

![img](../img/quickstart/write/tableWrite.png)

### 对象

参照：[最简单的写的对象](#最简单的写的对象)

### 代码

```java
    /**
     * 使用table去写入
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link DemoData}
     * <p>
     * 2. 然后写入table即可
     */
    @Test
    public void tableWrite() {
        String fileName = TestFileUtil.getPath() + "tableWrite" + System.currentTimeMillis() + ".xlsx";
        // 方法1 这里直接写多个table的案例了，如果只有一个 也可以直一行代码搞定，参照其他案
        // 这里 需要指定写用哪个class去写
        try (ExcelWriter excelWriter = EasyExcel.write(fileName, DemoData.class).build()) {
            // 把sheet设置为不需要头 不然会输出sheet的头 这样看起来第一个table 就有2个头了
            WriteSheet writeSheet = EasyExcel.writerSheet("模板").needHead(Boolean.FALSE).build();
            // 这里必须指定需要头，table 会继承sheet的配置，sheet配置了不需要，table 默认也是不需要
            WriteTable writeTable0 = EasyExcel.writerTable(0).needHead(Boolean.TRUE).build();
            WriteTable writeTable1 = EasyExcel.writerTable(1).needHead(Boolean.TRUE).build();
            // 第一次写入会创建头
            excelWriter.write(data(), writeSheet, writeTable0);
            // 第二次写如也会创建头，然后在第一次的后面写入数据
            excelWriter.write(data(), writeSheet, writeTable1);
        }
    }
```

## 动态头，实时生成头写入

### excel示例

![img](../img/quickstart/write/dynamicHeadWrite.png)

### 对象

参照：[最简单的写的对象](#最简单的写的对象)

### 代码

```java
    /**
     * 动态头，实时生成头写入
     * <p>
     * 思路是这样子的，先创建List<String>头格式的sheet仅仅写入头,然后通过table 不写入头的方式 去写入数据
     *
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link DemoData}
     * <p>
     * 2. 然后写入table即可
     */
    @Test
    public void dynamicHeadWrite() {
        String fileName = TestFileUtil.getPath() + "dynamicHeadWrite" + System.currentTimeMillis() + ".xlsx";
        EasyExcel.write(fileName)
            // 这里放入动态头
            .head(head()).sheet("模板")
            // 当然这里数据也可以用 List<List<String>> 去传入
            .doWrite(data());
    }

    private List<List<String>> head() {
        List<List<String>> list = new ArrayList<List<String>>();
        List<String> head0 = new ArrayList<String>();
        head0.add("字符串" + System.currentTimeMillis());
        List<String> head1 = new ArrayList<String>();
        head1.add("数字" + System.currentTimeMillis());
        List<String> head2 = new ArrayList<String>();
        head2.add("日期" + System.currentTimeMillis());
        list.add(head0);
        list.add(head1);
        list.add(head2);
        return list;
    }
```

## 自动列宽(不太精确)

### excel示例

![img](../img/quickstart/write/longestMatchColumnWidthWrite.png)

### 对象

```java
@Getter
@Setter
@EqualsAndHashCode
public class LongestMatchColumnWidthData {
    @ExcelProperty("字符串标题")
    private String string;
    @ExcelProperty("日期标题很长日期标题很长日期标题很长很长")
    private Date date;
    @ExcelProperty("数字")
    private Double doubleData;
}
```

### 代码

```java
   /**
     * 自动列宽(不太精确)
     * <p>
     * 这个目前不是很好用，比如有数字就会导致换行。而且长度也不是刚好和实际长度一致。 所以需要精确到刚好列宽的慎用。 当然也可以自己参照
     * {@link LongestMatchColumnWidthStyleStrategy}重新实现.
     * <p>
     * poi 自带{@link SXSSFSheet#autoSizeColumn(int)} 对中文支持也不太好。目前没找到很好的算法。 有的话可以推荐下。
     *
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link LongestMatchColumnWidthData}
     * <p>
     * 2. 注册策略{@link LongestMatchColumnWidthStyleStrategy}
     * <p>
     * 3. 直接写即可
     */
    @Test
    public void longestMatchColumnWidthWrite() {
        String fileName =
            TestFileUtil.getPath() + "longestMatchColumnWidthWrite" + System.currentTimeMillis() + ".xlsx";
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        EasyExcel.write(fileName, LongestMatchColumnWidthData.class)
            .registerWriteHandler(new LongestMatchColumnWidthStyleStrategy()).sheet("模板").doWrite(dataLong());
    }

    private List<LongestMatchColumnWidthData> dataLong() {
        List<LongestMatchColumnWidthData> list = new ArrayList<LongestMatchColumnWidthData>();
        for (int i = 0; i < 10; i++) {
            LongestMatchColumnWidthData data = new LongestMatchColumnWidthData();
            data.setString("测试很长的字符串测试很长的字符串测试很长的字符串" + i);
            data.setDate(new Date());
            data.setDoubleData(1000000000000.0);
            list.add(data);
        }
        return list;
    }
```

## 自定义拦截器（上面几点都不符合但是要对单元格进行操作的参照这个）

### excel示例

![img](../img/quickstart/write/customHandlerWrite.png)

### 对象

参照：[最简单的写的对象](#最简单的写的对象)

### 定义拦截器

```java
/**
 * 自定义拦截器。对第一行第一列的头超链接到:https://github.com/alibaba/easyexcel
 *
 * @author Jiaju Zhuang
 */
@Slf4j
public class CustomCellWriteHandler implements CellWriteHandler {

    @Override
    public void afterCellDispose(CellWriteHandlerContext context) {
        Cell cell = context.getCell();
        // 这里可以对cell进行任何操作
        log.info("第{}行，第{}列写入完成。", cell.getRowIndex(), cell.getColumnIndex());
        if (BooleanUtils.isTrue(context.getHead()) && cell.getColumnIndex() == 0) {
            CreationHelper createHelper = context.getWriteSheetHolder().getSheet().getWorkbook().getCreationHelper();
            Hyperlink hyperlink = createHelper.createHyperlink(HyperlinkType.URL);
            hyperlink.setAddress("https://github.com/alibaba/easyexcel");
            cell.setHyperlink(hyperlink);
        }
    }

}
```

```java
/**
 * 自定义拦截器.对第一列第一行和第二行的数据新增下拉框，显示 测试1 测试2
 *
 * @author Jiaju Zhuang
 */
@Slf4j
public class CustomSheetWriteHandler implements SheetWriteHandler {

    @Override
    public void afterSheetCreate(SheetWriteHandlerContext context) {
        log.info("第{}个Sheet写入成功。", context.getWriteSheetHolder().getSheetNo());

        // 区间设置 第一列第一行和第二行的数据。由于第一行是头，所以第一、二行的数据实际上是第二三行
        CellRangeAddressList cellRangeAddressList = new CellRangeAddressList(1, 2, 0, 0);
        DataValidationHelper helper = context.getWriteSheetHolder().getSheet().getDataValidationHelper();
        DataValidationConstraint constraint = helper.createExplicitListConstraint(new String[] {"测试1", "测试2"});
        DataValidation dataValidation = helper.createValidation(constraint, cellRangeAddressList);
        context.getWriteSheetHolder().getSheet().addValidationData(dataValidation);
    }
}
```

### 代码

```java
    /**
     * 下拉，超链接等自定义拦截器（上面几点都不符合但是要对单元格进行操作的参照这个）
     * <p>
     * demo这里实现2点。1. 对第一行第一列的头超链接到:https://github.com/alibaba/easyexcel 2. 对第一列第一行和第二行的数据新增下拉框，显示 测试1 测试2
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link DemoData}
     * <p>
     * 2. 注册拦截器 {@link CustomCellWriteHandler} {@link CustomSheetWriteHandler}
     * <p>
     * 2. 直接写即可
     */
    @Test
    public void customHandlerWrite() {
        String fileName = TestFileUtil.getPath() + "customHandlerWrite" + System.currentTimeMillis() + ".xlsx";
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        EasyExcel.write(fileName, DemoData.class).registerWriteHandler(new CustomSheetWriteHandler())
            .registerWriteHandler(new CustomCellWriteHandler()).sheet("模板").doWrite(data());
    }
```

## 插入批注

### excel示例

![img](../img/quickstart/write/commentWrite.png)

### 对象

参照：[最简单的写的对象](#最简单的写的对象)

### 定义拦截器

```java
/**
 * 自定义拦截器.新增注释,第一行头加批注
 *
 * @author Jiaju Zhuang
 */
@Slf4j
public class CommentWriteHandler implements RowWriteHandler {

    @Override
    public void afterRowDispose(RowWriteHandlerContext context) {
        if (BooleanUtils.isTrue(context.getHead())) {
            Sheet sheet = context.getWriteSheetHolder().getSheet();
            Drawing<?> drawingPatriarch = sheet.createDrawingPatriarch();
            // 在第一行 第二列创建一个批注
            Comment comment =
                drawingPatriarch.createCellComment(new XSSFClientAnchor(0, 0, 0, 0, (short)1, 0, (short)2, 1));
            // 输入批注信息
            comment.setString(new XSSFRichTextString("创建批注!"));
            // 将批注添加到单元格对象中
            sheet.getRow(0).getCell(1).setCellComment(comment);
        }
    }

}

```

### 代码

```java
    /**
     * 插入批注
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link DemoData}
     * <p>
     * 2. 注册拦截器 {@link CommentWriteHandler}
     * <p>
     * 2. 直接写即可
     */
    @Test
    public void commentWrite() {
        String fileName = TestFileUtil.getPath() + "commentWrite" + System.currentTimeMillis() + ".xlsx";
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        // 这里要注意inMemory 要设置为true，才能支持批注。目前没有好的办法解决 不在内存处理批注。这个需要自己选择。
        EasyExcel.write(fileName, DemoData.class).inMemory(Boolean.TRUE).registerWriteHandler(new CommentWriteHandler())
            .sheet("模板").doWrite(data());
    }
```

## 可变标题处理(包括标题国际化等)

### excel示例

![img](../img/quickstart/write/variableTitleWrite.png)

### 对象

```java
@Getter
@Setter
@EqualsAndHashCode
public class ConverterData {
    /**
     * 我想所有的 字符串起前面加上"自定义："三个字
     */
    @ExcelProperty(value = "字符串标题", converter = CustomStringStringConverter.class)
    private String string;
    /**
     * 我想写到excel 用年月日的格式
     */
    @DateTimeFormat("yyyy年MM月dd日HH时mm分ss秒")
    @ExcelProperty("日期标题")
    private Date date;
    /**
     * 我想写到excel 用百分比表示
     */
    @NumberFormat("#.##%")
    @ExcelProperty(value = "数字标题")
    private Double doubleData;
}
```

### 代码

```java
    /**
     * 可变标题处理(包括标题国际化等)
     * <p>
     * 简单的说用List<List<String>>的标题 但是还支持注解
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link ConverterData}
     * <p>
     * 2. 直接写即可
     */
    @Test
    public void variableTitleWrite() {
        // 写法1
        String fileName = TestFileUtil.getPath() + "variableTitleWrite" + System.currentTimeMillis() + ".xlsx";
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        EasyExcel.write(fileName, ConverterData.class).head(variableTitleHead()).sheet("模板").doWrite(data());
    }

    private List<List<String>> variableTitleHead() {
        List<List<String>> list = ListUtils.newArrayList();
        List<String> head0 = ListUtils.newArrayList();
        head0.add("string" + System.currentTimeMillis());
        List<String> head1 = ListUtils.newArrayList();
        head1.add("number" + System.currentTimeMillis());
        List<String> head2 = ListUtils.newArrayList();
        head2.add("date" + System.currentTimeMillis());
        list.add(head0);
        list.add(head1);
        list.add(head2);
        return list;
    }    private List<List<String>> variableTitleHead() {
        List<List<String>> list = new ArrayList<>();
        List<String> head0 = new ArrayList<>();
        head0.add("string" + System.currentTimeMillis());
        List<String> head1 = new ArrayList<>();
        head1.add("number" + System.currentTimeMillis());
        List<String> head2 = new ArrayList<>();
        head2.add("date" + System.currentTimeMillis());
        list.add(head0);
        list.add(head1);
        list.add(head2);
        return list;
    }
```

## 不创建对象的写

### excel示例

![img](../img/quickstart/write/simpleWrite.png)

### 代码

```java
    /**
     * 不创建对象的写
     */
    @Test
    public void noModelWrite() {
        // 写法1
        String fileName = TestFileUtil.getPath() + "noModelWrite" + System.currentTimeMillis() + ".xlsx";
        // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
        EasyExcel.write(fileName).head(head()).sheet("模板").doWrite(dataList());
    }

    private List<List<String>> head() {
        List<List<String>> list = ListUtils.newArrayList();
        List<String> head0 = ListUtils.newArrayList();
        head0.add("字符串" + System.currentTimeMillis());
        List<String> head1 = ListUtils.newArrayList();
        head1.add("数字" + System.currentTimeMillis());
        List<String> head2 = ListUtils.newArrayList();
        head2.add("日期" + System.currentTimeMillis());
        list.add(head0);
        list.add(head1);
        list.add(head2);
        return list;
    }

    private List<List<Object>> dataList() {
        List<List<Object>> list = ListUtils.newArrayList();
        for (int i = 0; i < 10; i++) {
            List<Object> data = ListUtils.newArrayList();
            data.add("字符串" + i);
            data.add(0.56);
            data.add(new Date());
            list.add(data);
        }
        return list;
    }
```

## web中的写

### 示例代码

DEMO代码地址：[https://github.com/alibaba/easyexcel/blob/master/easyexcel-test/src/test/java/com/alibaba/easyexcel/test/demo/web/WebTest.java](https://github.com/alibaba/easyexcel/blob/master/easyexcel-test/src/test/java/com/alibaba/easyexcel/test/demo/web/WebTest.java)

### 对象

参照：[最简单的写的对象](#最简单的写的对象) 就是名称变了下

### 代码

```java
    /**
     * 文件下载（失败了会返回一个有部分数据的Excel）
     * <p>
     * 1. 创建excel对应的实体对象 参照{@link DownloadData}
     * <p>
     * 2. 设置返回的 参数
     * <p>
     * 3. 直接写，这里注意，finish的时候会自动关闭OutputStream,当然你外面再关闭流问题不大
     */
    @GetMapping("download")
    public void download(HttpServletResponse response) throws IOException {
        // 这里注意 有同学反应使用swagger 会导致各种问题，请直接用浏览器或者用postman
        response.setContentType("application/vnd.openxmlformats-officedocument.spreadsheetml.sheet");
        response.setCharacterEncoding("utf-8");
        // 这里URLEncoder.encode可以防止中文乱码 当然和easyexcel没有关系
        String fileName = URLEncoder.encode("测试", "UTF-8").replaceAll("\\+", "%20");
        response.setHeader("Content-disposition", "attachment;filename*=utf-8''" + fileName + ".xlsx");
        EasyExcel.write(response.getOutputStream(), DownloadData.class).sheet("模板").doWrite(data());
    }
```

## web中的写并且失败的时候返回json

### since

2.1.1

### 对象

参照：[最简单的写的对象](#最简单的写的对象) 就是名称变了下

### 代码

```java
    /**
     * 文件下载并且失败的时候返回json（默认失败了会返回一个有部分数据的Excel）
     *
     * @since 2.1.1
     */
    @GetMapping("downloadFailedUsingJson")
    public void downloadFailedUsingJson(HttpServletResponse response) throws IOException {
        // 这里注意 有同学反应使用swagger 会导致各种问题，请直接用浏览器或者用postman
        try {
            response.setContentType("application/vnd.openxmlformats-officedocument.spreadsheetml.sheet");
            response.setCharacterEncoding("utf-8");
            // 这里URLEncoder.encode可以防止中文乱码 当然和easyexcel没有关系
            String fileName = URLEncoder.encode("测试", "UTF-8").replaceAll("\\+", "%20");
            response.setHeader("Content-disposition", "attachment;filename*=utf-8''" + fileName + ".xlsx");
            // 这里需要设置不关闭流
            EasyExcel.write(response.getOutputStream(), DownloadData.class).autoCloseStream(Boolean.FALSE).sheet("模板")
                .doWrite(data());
        } catch (Exception e) {
            // 重置response
            response.reset();
            response.setContentType("application/json");
            response.setCharacterEncoding("utf-8");
            Map<String, String> map = MapUtils.newHashMap();
            map.put("status", "failure");
            map.put("message", "下载文件失败" + e.getMessage());
            response.getWriter().println(JSON.toJSONString(map));
        }
    }
```




