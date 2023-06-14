---
title: "React展示PDF或图片资源"
catalog: true
date: 2023-04-25 21:37:19
subtitle: "写项目时候遇到的需求"
header-img: 
tags:
- React
- JavaScript
catagories:
- React
---
### 笔记描述
> 后台接口返回png文件或者PDF文件需要展示
### 主要代码
```jsx
// 定义文件类型
 const blobType = {
    pdf: "application/pdf",
    png: "image/png",
    jpeg: "image/jpeg",
    jpg: "image/jpeg"
  }
// 获取文件用FileReader转格式
  const showFile = async (record) => {
    setFileId(record.fileId)
    await request(`/api/file/fileDown/downloadFileById?fileId=${record.fileId}`, {
      method: "get",
      responseType: "blob"
    }).then((res) => {
      var reader = new FileReader();
      // 判断文件类型
      const type = record.fileName.split(".");
      let blob = new Blob([res], { type: blobType[type[type.length - 1]] });
      if (blob) {
        reader.readAsDataURL(blob)
      }
      reader.addEventListener("load", function () {
        setFileUrl(reader.result)
        setShowModal(true)
      }, false);
      reader.addEventListener("error", () => {
        message.error("获取文件失败！")
      })
    })
  }

```
### 展示
```html
   <iframe src={fileUrl} width="1200" height="800" style={{ border: "none" }} />
```