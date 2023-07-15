

## 一.项目架构

#### 1.以天通项目为例，这是最基本的项目结构

![image-20230625162919780](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230625162919780.png)

#### 2.app模块

![image-20230625163532182](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230625163532182.png)

## 二、文件存贮

#### 1、Shared Preferences存储

键值对形式，有三种方式来获取Shared Preferences对象，最常用的方式为Context类中的getSharedPreferences()方法，这个方法接收两个参数，第一个为**文件名称**，不存在的会自动创建，该文件一般都存放在/data/data/<package name>/shared_prefs/目录下，第二个方法为操作模式，只有**MODE_PRIVATE**一种模式。

以下为最经典的使用方式

~~~java
SharedPreferences.Editor editor = getSharedPreferences("data", MODE_PRIVATE).edit();
edit.putString("name","tom");//添加数据
editor.apply();//提交数据，完成存储
~~~

读取的时候通过

~~~java
SharedPreferences pref = getSharedPreferences("data", MODE_PRIVATE).edit();
String name = pref.getString("name", "");//根据键获取值
~~~

## 三、调用API流程

#### 1、向excel中写入数据

在的Android项目中添加了Apache POI库，在项目的 build.gradle 文件中添加以下依赖：

```groovy
implementation group: 'org.apache.poi', name: 'poi-ooxml', version: '5.2.2'
```

然后，在项目的 build.gradle 文件中添加以下依赖：

~~~groovy
<uses-permission
        android:name="android.permission.WRITE_EXTERNAL_STORAGE"
        tools:ignore="ScopedStorage" /> <!-- 允许程序设置内置sd卡的写权限 -->
~~~

最后根据对应的api中的方法进行操作，以本项目为例

~~~Java
/**
     * 初始化Excel表格
     *
     * @param filePath  存放excel文件的路径（path/demo.xls）
     * @param sheetName Excel表格的表名
     * @param colName   excel中包含的列名（可以有多个）
     */
    public static void initExcel(String filePath, String sheetName, String[] colName) throws WriteException, IOException {
        format();
        WritableWorkbook workbook = null;
        try {
            File file = new File(filePath);
            if (!file.exists()) {
                file.createNewFile();
            } else {
                return;
            }
            workbook = Workbook.createWorkbook(file);
            //设置表格的名字
            WritableSheet sheet = workbook.createSheet(sheetName, 0);
            //创建标题栏
            sheet.addCell((WritableCell) new Label(0, 0, filePath, arial14format));
            for (int col = 0; col < colName.length; col++) {
                sheet.addCell(new Label(col, 0, colName[col], arial10format));
            }
            //设置行高
            sheet.setRowView(0, 340);
            workbook.write();
        } catch (Exception e) {
            throw e;
        } finally {
            if (workbook != null) {
                try {
                    workbook.close();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }
    }
~~~

~~~Java
/**
     * 将Alarm的List写入Excel中
     *
     * @param objList  待写入的list
     * @param fileName
     * @param mContext
     * @param <T>
     */
    @SuppressWarnings("unchecked")
    public static <T> void writeAlarmToExcel(List<T> objList, String fileName, Context mContext) {
        if (objList != null && objList.size() > 0) {
            WritableWorkbook writebook = null;
            InputStream in = null;
            try {
                WorkbookSettings setEncode = new WorkbookSettings();
                setEncode.setEncoding(UTF8_ENCODING);

                in = new FileInputStream(new File(fileName));
                Workbook workbook = Workbook.getWorkbook(in);
                writebook = Workbook.createWorkbook(new File(fileName), workbook);
                WritableSheet sheet = writebook.getSheet(0);

                for (int j = 0; j < objList.size(); j++) {
                    Alarm bean = (Alarm) objList.get(j);
                    List<String> list = new ArrayList<>();
                    list.add(bean.getImsi());
                    list.add(ImsiTypeEnum.getByCode(bean.getImsiType()).getDesc());
                    if (bean.getPopulation() != null && bean.getPopulation().getPopName() != null) {
                        list.add(bean.getPopulation().getPopName());
                    } else {
                        list.add("姓名未知");
                    }
/*

                    if (bean.getDescription() != null) {
                        list.add(bean.getDescription());
                    } else {
                        list.add("暂无描述");
                    }

*/
                    list.add(DateUtil.format(bean.getAlarmTime(), "yyyy/MM/dd HH:mm:ss"));
/*
                    if (bean.getCurrentLocation() != null) {
                        list.add(bean.getCurrentLocation());
                    } else {
                        list.add("位置未知");
                    }
*/
                    for (int i = 0; i < list.size(); i++) {
                        sheet.addCell(new Label(i, j + 1, list.get(i), arial12format));
                        sheet.setColumnView(i, list.get(i).length() + 8);
//                        if (list.get(i).length() <= 4) {
//                            //设置列宽
//                            sheet.setColumnView(i, list.get(i).length() + 8);
//                        } else {
//                            //设置列宽
//                            sheet.setColumnView(i, list.get(i).length() + 5);
//                        }
                    }
                    //设置行高
                    sheet.setRowView(j + 1, 350);
                }

                writebook.write();
                workbook.close();
                Log.e(ExcelUtil.class.getName(), "导出Excel成功");
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                if (writebook != null) {
                    try {
                        writebook.close();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
                if (in != null) {
                    try {
                        in.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }

        }
    }
~~~

