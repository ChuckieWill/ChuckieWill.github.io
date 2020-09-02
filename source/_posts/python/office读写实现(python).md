---
title: office读写实现(python)
date: 2020-04-02 10:46:17
tags:
- python
categories:
- [python]
---

---
title: office读写实现(python)
date: 2020-02-02 10:46:17
tags:

- python
- 爬虫
categories:
- [python]
---

###  一、Excel

####  1.1 写入Excel

* 文件生成在执行文件同目录下

```py
import openpyxl  # 导入读写库

workbook = openpyxl.Workbook()
sheet = workbook.create_sheet("名称") # excel中sheet名称
# heet.cell(行-y轴,列-x轴,'内容')  向文件指定行列写入数据
sheet.cell(1,2,'哈哈哈')

workbook.save("文件名.xlsx")  //保存文件并命名文件
```

