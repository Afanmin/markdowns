---
title: 使用openpyxl包生成xlsx文件
date: 2020-10-09 17:52:17
tags:
---

### 需求简介

就，商务那边经常需要excel文件来做‘数据分析’，动不动就需要把数据库里筛选出来的东西放进表里。所以直接用python脚本生成xlsx文件会显得‘很方便‘。



### 代码样例

想了想这玩意也没啥原理可以说，就直接放代码，这个包支持图片，支持新建以及修改单元格分页，除了用excel画图编程什么的基本功能全部能够实现。

```python
from openpyxl import Workbook, load_workbook
wkbook = Workbook()
score_re = {}
for key,value in select_res_xizhuang.items():
    ws = wkbook.create_sheet()
    ws.title = key
    ws['A1'] = 'id'
    ws['B1'] = 'score'
    ws['C1'] = 'fanscount'
    num = 0
    total_score = 0
    for _,pid in enumerate(value):
        try:
            es_res = ES.get(index='instagram-posts',id=pid)
        except:
            continue
        # try:
        score = es_res['_source']['scoreValue']
        fans =  es_res['_source']['fansCount']
        total_score += score
        ws['A'+str(_+2)] = pid
        ws['B'+str(_+2)] = score
        ws['C'+str(_+2)] = fans
        num += 1

    if num > 0: 
        score_re[key] = total_score/num
wkbook.save('res.xlsx')
```

```python
from openpyxl import load_workbook
from openpyxl.drawing.image import Image


def insertimg2excel(imgPath, excelPath):
    imgsize = (720 / 4, 1280 / 4)  # 设置一个图像缩小的比例
    wb = load_workbook(excelPath)
    ws = wb.active
    ws.column_dimensions['A'].width = imgsize[0] * 0.14  # 修改列A的宽

    img = Image(imgPath)  # 缩放图片
    img.width, img.height = imgsize

    ws.add_image(img, 'A1')  # 图片 插入 A1 的位置上
    ws.row_dimensions[1].height = imgsize[1] * 0.78  # 修改列第1行的宽

    wb.save('out.xlsx')  # 新的结果保存输出


if __name__ == '__main__':
    imgPath = '001.jpg'
    excelPath = 'test.xlsx'
    insertimg2excel(imgPath, excelPath)

```

