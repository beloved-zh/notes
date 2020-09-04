# POIAndEasyExcel

## 应用场景

1. 将数据导出为excel表格（导出数据...）
2. 将excel数据保存在数据库

操作Excel比较流行的技术是 **Apache POI**和阿里巴巴的**esayExcel**

### Apache POI

官网：http://poi.apache.org/

![img](image-20200717183824526.png)

![img](image-20200717183919436.png)

### EasyExcel

官网：https://github.com/alibaba/easyexcel

官方文档：https://www.yuque.com/easyexcel/doc/easyexcel

![img](image-20200717184241343.png)

EsayExcel是阿里巴巴开源的一个excel处理框架，**使用简单、节省内存**

EsayExcel能大大减少占用内存的主要原因是在解析Excel时没有将文件数据一次性全部加载到内存中，而是从磁盘上一行行读取数据，逐个解析

内存问题：



EsayExcel与POI对比图

![img](image-20200717184410996.png)

## Excel分析

**Excel   03版本与07版本存在兼容 问题**

- 03
  - 最多只有65536行
  - 后缀   .xls
- 07
  - 行数没有限制
  - 后缀  .xlsx

Excel结构：

- 工作簿
- 工作表
- 行
- 列
- 单元格 

## POI写

### 导入依赖

```xml
<!-- xls(03) -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>3.9</version>
</dependency>

<!-- xlsx(07) -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>3.9</version>
</dependency>

<!-- 日期格式化工具 -->
<dependency>
    <groupId>joda-time</groupId>
    <artifactId>joda-time</artifactId>
    <version>2.10.5</version>
</dependency>

<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

### 03版本

```java
String PATH = "E:\\ideaProject\\POIAndEasyExcel\\POI";

/**
 * 03版本写入测试
 */
@Test
public void ExcelWrite03() throws IOException {
    //1.创建一个工作簿
    Workbook workbook = new HSSFWorkbook();
    //2.创建一个工作表
    Sheet sheet = workbook.createSheet("学生表");
    //3.创建一个行(第一行)
    Row row1 = sheet.createRow(0);
    //4.创建单元格
    //(1,1)
    Cell cell11 = row1.createCell(0);
    cell11.setCellValue("姓名");
    //(1,2)
    Cell cell12 = row1.createCell(1);
    cell12.setCellValue("张恒");

    //第二行
    Row row2 = sheet.createRow(1);
    //(2,1)
    Cell cell21 = row2.createCell(0);
    cell21.setCellValue("年龄");
    //(2,2)
    Cell cell22 = row2.createCell(1);
    cell22.setCellValue(20);

    //第三行
    Row row3 = sheet.createRow(2);
    //(3,1)
    Cell cell31 = row3.createCell(0);
    cell31.setCellValue("生日");
    //(3,2)
    Cell cell32 = row3.createCell(1);
    cell32.setCellValue(new DateTime().toString("yyyy-MM-dd HH:mm:ss"));

    //5.生成表（IO流）    03版本使用.xls结尾
    FileOutputStream fileOutputStream = new FileOutputStream(PATH + "学生表03.xls");

    //6.输出
    workbook.write(fileOutputStream);

    //7.关闭流
    fileOutputStream.close();

    System.out.println("生成完毕");
}
```

### 07版本

```java
String PATH = "E:\\ideaProject\\POIAndEasyExcel\\POI";

/**
 * 07版本写入测试
 */
@Test
public void ExcelWrite07() throws IOException {
    //1.创建一个工作簿
    Workbook workbook = new XSSFWorkbook();
    //2.创建一个工作表
    Sheet sheet = workbook.createSheet("学生表");
    //3.创建一个行(第一行)
    Row row1 = sheet.createRow(0);
    //4.创建单元格
    //(1,1)
    Cell cell11 = row1.createCell(0);
    cell11.setCellValue("姓名");
    //(1,2)
    Cell cell12 = row1.createCell(1);
    cell12.setCellValue("张恒");

    //第二行
    Row row2 = sheet.createRow(1);
    //(2,1)
    Cell cell21 = row2.createCell(0);
    cell21.setCellValue("年龄");
    //(2,2)
    Cell cell22 = row2.createCell(1);
    cell22.setCellValue(20);

    //第三行
    Row row3 = sheet.createRow(2);
    //(3,1)
    Cell cell31 = row3.createCell(0);
    cell31.setCellValue("生日");
    //(3,2)
    Cell cell32 = row3.createCell(1);
    cell32.setCellValue(new DateTime().toString("yyyy-MM-dd HH:mm:ss"));

    //5.生成表（IO流）    07版本使用.xlsx结尾
    FileOutputStream fileOutputStream = new FileOutputStream(PATH + "学生表07.xlsx");

    //6.输出
    workbook.write(fileOutputStream);

    //7.关闭流
    fileOutputStream.close();

    System.out.println("生成完毕");
}
```

**03与07版本区别就是对象不一样，文件名不一样，其余操作没有区别**

### 数据批量导入

#### 大文件写HSSF

缺点：最多只能处理65536行，否则会抛出异常

`java.lang.IllegalArgumentException: Invalid row number (65536) outside allowable range (0..65535)`

优点：过程中写入缓存，不操作磁盘，最后一次性写入磁盘，速度快

```java
/**
 * 03版本大数据写入测试
 */
@Test
public void ExcelWrite03BigData() throws IOException {
    //开始时间
    long begin = System.currentTimeMillis();

    //创建工作簿
    Workbook workbook = new HSSFWorkbook();
    //创建表
    Sheet sheet = workbook.createSheet();
    //写入数据
    for (int i = 0; i < 65536; i++) {
        Row row = sheet.createRow(i);
        for (int j = 0; j < 10; j++) {
            Cell cell = row.createCell(j);
            cell.setCellValue(j);
        }
    }

    System.out.println("over");

    FileOutputStream fileOutputStream = new FileOutputStream(PATH + "ExcelWrite03BigData.xls");

    workbook.write(fileOutputStream);

    fileOutputStream.close();

    //结束时间
    long end = System.currentTimeMillis();

    System.out.println((double) (end - begin) / 1000);
}
```

![img](image-20200717194736379.png)

#### 大文件写XSSF

缺点：写数据时速度非常慢，非常消耗内存，也会发生内存溢出异常，如100万条

优点：可以写入较大的数据，如20万条

```java
/**
 * 07版本大数据写入测试
 */
@Test
public void ExcelWrite07BigData() throws IOException {
    //开始时间
    long begin = System.currentTimeMillis();

    //创建工作簿
    Workbook workbook = new XSSFWorkbook();
    //创建表
    Sheet sheet = workbook.createSheet();
    //写入数据
    for (int i = 0; i < 100000; i++) {
        Row row = sheet.createRow(i);
        for (int j = 0; j < 10; j++) {
            Cell cell = row.createCell(j);
            cell.setCellValue(j);
        }
    }

    System.out.println("over");

    FileOutputStream fileOutputStream = new FileOutputStream(PATH + "ExcelWrite07BigData.xlsx");

    workbook.write(fileOutputStream);

    fileOutputStream.close();

    //结束时间
    long end = System.currentTimeMillis();

    System.out.println((double) (end - begin) / 1000);
}
```

#### 大文件写SXSSF

优点：可以写非常大的数据，写数据速度快，占用更少内存

**注意：**

- 过程会产生临时文件，需要清理临时文件
- 默认由100条记录被保存在内存中，如果超过这数量，则最前面的数据被写入临时文件
- 如果想自定义内存中数据的数量，可以使用new SXSSFWorkbook(数量)

```java
/**
 * SXSSF大数据写入测试
 */
@Test
public void ExcelWrite07BigDataS() throws IOException {
    //开始时间
    long begin = System.currentTimeMillis();

    //创建工作簿
    Workbook workbook = new SXSSFWorkbook();
    //创建表
    Sheet sheet = workbook.createSheet();
    //写入数据
    for (int i = 0; i < 100000; i++) {
        Row row = sheet.createRow(i);
        for (int j = 0; j < 10; j++) {
            Cell cell = row.createCell(j);
            cell.setCellValue(j);
        }
    }

    System.out.println("over");

    FileOutputStream fileOutputStream = new FileOutputStream(PATH + "ExcelWrite07BigDataS.xlsx");

    workbook.write(fileOutputStream);

    fileOutputStream.close();

    //清除临时文件
    ((SXSSFWorkbook)workbook).dispose();

    //结束时间
    long end = System.currentTimeMillis();

    System.out.println((double) (end - begin) / 1000);
}
```

**注意：清理临时文件**

SXSSFWorkbook来至官方解释：实现“BigGridDemo”策略的流式XSSFWorkbook版本。这允许写入非常大的文件而不会耗尽内存，因

为任何时候只有可配置的行部分被保存在内存中

请注意，任然可能会消耗大量内存，这些内存基于正使用的功能，例如合并区域，注释......任然只能存储在内存中，因此如果广泛使用，可能需要大量内存

## POI读

### 简单的读取

#### 03版本

```java
/**
 * 03简单的读操作
 */
@Test
public void ExcelRead03() throws IOException {


    String PATH = "E:\\ideaProject\\POIAndEasyExcel\\POI学生表.xls";

    //获取文件流
    FileInputStream inputStream = new FileInputStream(PATH);

    //创建工作簿  使用excel能操作这里都可以操作
    Workbook workbook = new HSSFWorkbook(inputStream);
    //根据下标获取工作表  也可以根据名
    Sheet sheet = workbook.getSheetAt(0);
    //获取行
    Row row = sheet.getRow(1);
    //获取列
    Cell cell = row.getCell(1);
    //获取值
    System.out.println(cell.getStringCellValue());
    inputStream.close();
}
```

#### 07版本

```java
/**
 * 07简单的读操作
 */
@Test
public void ExcelRead07() throws IOException {


    String PATH = "E:\\ideaProject\\POIAndEasyExcel\\POI学生表07.xlsx";

    //获取文件流
    FileInputStream inputStream = new FileInputStream(PATH);

    //创建工作簿  使用excel能操作这里都可以操作
    Workbook workbook = new XSSFWorkbook(inputStream);
    //根据下标获取工作表  也可以根据名
    Sheet sheet = workbook.getSheetAt(0);
    //获取行
    Row row = sheet.getRow(2);
    //获取列
    Cell cell = row.getCell(1);
    //获取值
    System.out.println(cell.getStringCellValue());
    inputStream.close();
}
```

**注意获取值得类型。类型不一致会报错**

![img](image-20200717210616522.png)

### 读取不同的数据类型

```java
/**
 * 测试读取不同的类型
 */
@Test
public void CellType() throws IOException {
    String PATH = "E:\\ideaProject\\POIAndEasyExcel\\明细表.xlsx";


    //获取文件流
    FileInputStream inputStream = new FileInputStream(PATH);

    //创建工作簿
    Workbook workbook = new XSSFWorkbook(inputStream);

    //获取表
    Sheet sheet = workbook.getSheetAt(0);

    // 获取标题内容
    // 获取第一行
    Row title = sheet.getRow(0);
    if (title != null){
        // 获取这一行有多少列
        int cellCount = title.getPhysicalNumberOfCells();
        for (int i = 0; i < cellCount; i++) {
            // 获取列
            Cell cell = title.getCell(i);
            if (cell!=null){
                // 获取类型
                int cellType = cell.getCellType();
                // 获取值
                String cellValue = cell.getStringCellValue();
                System.out.print(cellValue + " | ");
            }
        }
        System.out.println();
    }

    // 获取表中的内容
    // 获取所有行
    int rowCount = sheet.getPhysicalNumberOfRows();
    for (int rowNum = 1; rowNum < rowCount; rowNum++) {
        // 获取行
        Row row = sheet.getRow(rowNum);
        if (row!=null){
            // 获取这一行有多少列
            int cellCount = title.getPhysicalNumberOfCells();
            for (int cellNum = 0; cellNum < cellCount; cellNum++) {
                System.out.print("[" + (rowNum+1) + "-" + (cellNum+1) + "]");
                // 读取列
                Cell cell = row.getCell(cellNum);
                // 匹配列的数据类型
                if (cell != null){
                    int cellType = cell.getCellType();

                    switch (cellType){
                        case XSSFCell.CELL_TYPE_STRING: // 字符串
                            System.out.print(" 字符串 ");
                            System.out.println(cell.getStringCellValue());
                            break;
                        case XSSFCell.CELL_TYPE_BOOLEAN: // 布尔类型
                            System.out.print(" 布尔类型 ");
                            System.out.println(cell.getBooleanCellValue());
                            break;
                        case XSSFCell.CELL_TYPE_BLANK: // 空值
                            System.out.println(" 空值 ");
                            break;
                        case XSSFCell.CELL_TYPE_NUMERIC: // 数字（日期、普通数字）
                            if (HSSFDateUtil.isCellDateFormatted(cell)){  // 日期
                                Date date = cell.getDateCellValue();
                                System.out.print(" 日期 ");
                                System.out.println(new DateTime(date).toString("yyyy-MM-dd HH:mm:ss"));
                            }else{
                                // 如果不是日期格式，防止数字过长,设置为字符串 可以读取科学计数法
                                cell.setCellType(XSSFCell.CELL_TYPE_STRING);
                                System.out.print(" 数字 ");
                                System.out.println(cell.getStringCellValue());
                            }
                            break;
                        case XSSFCell.CELL_TYPE_ERROR: // 错误
                            System.out.println("数据类型错误 ");
                            break;
                    }
                }
            }
        }
    }

    inputStream.close();
}
```

## EasyExcel

官方文档具有详细操作说明

官方文档：https://www.yuque.com/easyexcel/doc/easyexcel

代码：https://github.com/beloved-zh/POIAndEasyExcel/tree/master

