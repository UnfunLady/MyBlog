---
title: "XLXS导入JSON识别"
catalog: true
date: 2025-02-03 21:12:30
subtitle: "识别excel、csv的json数据"
header-img:
tags:
  - xlxs
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于Antd upload组件上传前识别

### 主要代码

```js
  beforeUpload: (data) => {
    $props({
      loading: true
    })
    const file = data
    const types = file.name.split('.')[1]
    const fileType = ['xlsx', 'xlc', 'xlm', 'xls', 'xlt', 'csv'].some(
      (item) => item === types
    )
    if (!fileType) {
      $message.error('格式错误！请选择xlsx、xlc、xlm、xls、xlt、csv格式文件',)
      return false;
    }
    const reader = new FileReader()
    if (types == 'csv') {
      reader.readAsText(file, 'utf-8');
    } else {
      reader.readAsArrayBuffer(file, 'utf-8');
    }
    new Promise((resolve, reject) => {
      reader.onloadend = (e) => {
        const data = e.target.result
        const wb = $xlsx.read(data, {
          type: types == 'csv' ? 'string' : 'buffer',
        })
        const ws = wb.Sheets[wb.SheetNames[0]]
        let arr = $xlsx.utils.sheet_to_json(ws);
        $props({
          loading: false
        })
        if (arr && arr.length) {
          let finalData = []
          arr.map((i) => {
            let newData = {}
            if (i["英文名"]) {
              newData['englishName'] = i["英文名"]
            }
            if (i["中文名"]) {
              newData['chineseName'] = i["中文名"]
            }
            if (i["必填"]) {
              newData['ifFillIn'] = i["必填"] == '是' ? '1' : '0'
            }
            if (i["类型"]) {
              switch (i["类型"]) {
                case "字符":
                  newData['fieldsType'] = "string"
                  break;
                case "字符串":
                  newData['fieldsType'] = "string"
                  break;
                case "布尔":
                  newData['fieldsType'] = "boolean"
                  break;
                case "文本":
                  newData['fieldsType'] = "text"
                  break;
                case "文本大对象":
                  newData['fieldsType'] = "text"
                  break;
                case "数字":
                  newData['fieldsType'] = "number"
                  break;
                case "字节":
                  newData['fieldsType'] = "byte"
                  break;
                case "字节大对象":
                  newData['fieldsType'] = "byte"
                  break;
              }
            }
            if (JSON.stringify(newData) != "{}") {
              finalData.push(newData)
            }
          })
          if (finalData.length) {
            $form.setValues({
              add: {
                ...$values.add,
                columnList: finalData
              }
            })
          } else {
            if (types == 'csv') {
              $message.warn("未识别出数据 请检查csv文件编码是否为UTF-8")
            } else {
              $message.warn("未识别出数据")
            }
            $form.setValues({
              add: {
                ...$values.add,
                columnList: []
              }
            })
          }
        } else {
          if (types == 'csv') {
            $message.warn("未识别出数据 请检查csv文件编码是否为UTF-8")
          } else {
            $message.warn("未识别出数据")
          }
        }
      }
    });
    return false;

  }
```
