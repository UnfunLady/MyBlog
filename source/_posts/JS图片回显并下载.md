---
title: "JS图片回显并下载"
catalog: true
date: 2022-11-02 15:11:46
subtitle: "学习经验."
header-img: 
tags:
- JavaScript
catagories:
- JS
---

### 笔记描述
>  对input输入的图片回显并下载

### 详细代码
 ```JavaScript
imginput.onchange = (e) => {
  const fileRead = new FileReader();
  fileRead.onload = () => {
    const img = document.createElement("img");
    // 设置图片地址
    img.setAttribute("src", fileRead.result)
    document.body.appendChild(img);
  }
  fileRead.readAsDataURL(e.target.files[0])
  // 生成blob对象
  const blob = new Blob([e.target.files[0]]);
  const a = document.createElement("a");
  a.setAttribute("download", "");
  // 生成a标签 并实现下载
  a.href = URL.createObjectURL(blob);
  a.click()
  URL.revokeObjectURL(blob)
}

 ```





