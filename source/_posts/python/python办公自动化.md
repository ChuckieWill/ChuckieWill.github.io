---
title: python办公自动化
date: 2020-04-02 10:46:17
tags:
- python
categories:
- [python]
---

#  pip

> 以下命令可在cmd终端执行

* `pip list`: 查看已安装的第三方库 
* `pip install ...`: 安装第三方库

# 一、写入Excel

###  1.1 xlrd模块(读取excel)

> xlrd为python的第三方库，模块用来读取Excel表格数据
>
> 只支持读取xls格式，不支持读取xlsx文件

* 安装xlrd: `pip install xlrd`

####  1.1.1 常用函数

```python
import xlrd

# 打开文件，xlrd不支持打开xlsx文件，可以绝对路径也可以相对路径
data=xlrd.open_workbook("data2.xls")
# 获取工作表是否加载完成，完成返回true否则返回false,
# 0为sheet1的索引，可以索引查询也可以工作表名称查询
print(data.sheet_loaded(0))
# 卸载工作表，将加载的工作表移除操作栈
# 0为sheet1的索引，可以索引卸载也可以工作表名称卸载
data.unload_sheet(0)
print(data.sheet_loaded (0))
print(data.sheet_loaded(1))

输出
True
False     
True   

print(data.sheets()) # 获取全部sheet
print(data.sheets()[0]) # 获取索引为0的工作表
print(data.sheet_by_index(0)) # 根据索引获取工作表
print(data.sheet_by_name("Sheet1")) # 根据sheetname进行获取工作表
print(data.sheet_names()) #获取所有工作表的name
print(data.nsheets) # 返回excle工作表的数量
```

####  1.1.2 操作excel行

```python
sheet=data.sheet_by_index(1)#获取第2个工作表
print(sheet.nrows)#获取sheet下的有效行数
print(sheet.row(1))#该行单元格对象组成的列表----获取第2行
print(sheet.row_types(2))#获取单元格的数据类型----获取第3行每个单元格的数据类型（1为文本，2为数字，，，）
print(sheet.row(1)[2].value)#得到单元格value-----获取第2行第3列单元格的值
print(sheet.row_values(1))#得到指定行单元格的value----获取第2行所有单元格的值，返回值为列表
print(sheet.row_len(1))#得到单元格的长度----获取第2行的有效单元格个数
```

####  1.1.3 操作excel列

```python
sheet=data.sheet_by_index(0)#获取第一个工作表
print (sheet.ncols)#获取sheet下的有效列数
print (sheet.col(1))#该列单元格对象组成的列表----获取第2列数据
print (sheet.col(1)[2].value)#---获取第2列第3行的值
print (sheet.col_values(1)) #返回该列所有单元格的value组成的列表---获取第2列所有的值
print (sheet.col_types(5))# 获取第6列的所有单元格数据类型
```

####  1.1.4 操作excel单元格

```python
sheet=data.sheet_by_index(0)#获取第一个工作表
print(sheet.cell(1,2))#获取第2行第3列的单元格对象
print(sheet.cell_type(1, 2)) #获取第2行第3列的单元格对象的数据类型
print(sheet.cell(1, 2).ctype)#获取第2行第3列的单元格对象的数据类型
print(sheet.cell(1, 2).value)#获取第2行第3列的单元格对象的值
print(sheet.cell_value(1, 2))#获取第2行第3列的单元格对象的值
```

###  1.2 xlwt模块(写入excel)

>xlwt为Python第三方模块,用来写入Excel表格数据
>
>只支持写入xls格式，不支持写入xlsx文件

* 安装xlwt: `pip install xlwt`

####  1.2.1 写入excel操作

```python
import xlwt
#第一步:创建工作簿
wb=xlwt.Workbook()
#第二步:创建工作表
ws=wb.add_sheet("CNY")
#第三步:填充数据
#将0-1行,0-5列合并,内容为：2019年货币兑换表
ws.write_merge(0, 1, 0, 5,"2019年货币兑换表")

#循环写入每个单元格数据
data = (("Date", "英镑","人民币","港币","日元","美元"
),("01/01/2019", 8.722551, 1, 0.877885, 0.062722, 6.8759
),("02/01/2019", 8.634922, 1, 0.875731, 0.062773, 6.8601
))
for i, item in enumerate(data):
  for j, val in enumerate(item):
    ws.write(i+2, j, val) #写入i+2行j列，写入内容为val

#第四步:保存
wb.save("2019CNY.xls")
```

#### 1.2.2 数据样式

```python
import xlwt

 #实例化样式对象，titlestyle为标题样式对象
titlestyle=xlwt.XFStyle()

#设置字体样式
titlefont=xlwt.Font()
titlefont.name="宋体"#字体样式
titlefont.bold=True #加粗
titlefont.height=11*20 #字号，字号为11,20为固定值
titlefont.colour_index=0x08 #字体颜色
titlestyle.font=titlefont

#单元格对齐方式
cellalign=xlwt.Alignment()
cellalign.horz=0x02#水平居中
cellalign.vert=0x01#上下居中
titlestyle.alignment=cellalign

#边框
borders=xlwt.Borders ()
borders.right=xlwt.Borders.DASHED#有边框为虚线
borders.bottom=xlwt.Borders.DOTTED#下边框为点线
titlestyle.borders=borders


#实例化样式对象，datestyle为某列或行或单元格样式对象
datestyle = xlwt.XFStyle()

#背景颜色
bgcolor = xlwt.Pattern()
bgcolor.pattern=xlwt.Pattern.SOLID_PATTERN#背景模式
bgcolor.pattern_fore_colour=22 #背景颜色
datestyle.pattern=bgcolor


#第一步:创建工作簿
wb=xlwt.Workbook()
#第二步:创建工作表
ws = wb.add_sheet("CNY")

#第三步:填充数据
#将0-1行,0-5列合并,内容为：2019年货币兑换表,titlestyle数据样式对象，已在上面定义
ws.write_merge(0, 1, 0, 5,"2019年货币兑换表",titlestyle)


data = (("Date", "英镑","人民币","港币","日元","美元"
),("01/01/2019", 8.722551, 1, 0.877885, 0.062722, 6.8759
),("02/01/2019", 8.634922, 1, 0.875731, 0.062773, 6.8601
))
for i, item in enumerate(data):
  for j, val in enumerate(item):
    if j == 0 :
      ws.write(i+2,j,val,datestyle)#datestyle数据样式对象，已在上面定义
    else:
      ws.write(i+2, j, val) #写入i+2行j列，写入内容为val


#第四步:保存
wb.save("2019CNY.xls")
```



* 文件生成在执行文件同目录下

```py
import openpyxl  # 导入读写库

workbook = openpyxl.Workbook()
sheet = workbook.create_sheet("名称") # excel中sheet名称
# heet.cell(行-y轴,列-x轴,'内容')  向文件指定行列写入数据
sheet.cell(1,2,'哈哈哈')

workbook.save("文件名.xlsx")  //保存文件并命名文件
```

