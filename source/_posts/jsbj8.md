---
title: "JS对图片压缩"
catalog: true
date: 2022-12-27 11:29:44
subtitle: "学习经验."
header-img: 
tags:
- JavaScript
catagories:
- JS
---

### 笔记描述
>  对选择上传的图片进行压缩 组件库则把获取到的file替换e.target.files[0]

### 详细代码
 ```HTML
<!DOCTYPE html>
<html lang="en">


  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  </head>


  <body>
    <input type="file" name="" id="file">
    <script>
      file.onchange = async function (e) {
        // 获取blob对象
        const f = await imgComporess(e.target.files[0])
        const img = document.createElement("img");
        // 生成地址并显示
        img.src = URL.createObjectURL(f);
        document.body.appendChild(img)
      }
      function imgToFile(file) {
        const fileReader = new FileReader()
        const img = new Image()
        return new Promise((res, rej) => {
          fileReader.onload = function () {
            img.src = this.result;
          }
          fileReader.readAsDataURL(file);
          img.onload = function () {
            res(img)
          }


        })
      }
      async function imgComporess(file) {
        const img = await imgToFile(file);
        return new Promise((res, rej) => {
          // 定义画布
          const canvas = document.createElement("canvas");
          const context = canvas.getContext("2d");
          const { width: originWidth, height: originHeight } = img;
          // 目标图片大小 越小压缩越严重
          const targetWidth = 600;
          // 缩放比例 
          const scale = (originWidth / originHeight).toFixed(2)
          const targetHeignt = targetWidth / scale;
          // 判断图片原大小是否超出默认的大小
          if (originWidth > targetWidth) {
            canvas.width = targetWidth;
            canvas.height = targetHeignt;
          } else {
            canvas.width = originWidth;
            canvas.height = originHeight;
          }
          // 绘制图片
          context.clearRect(0, 0, canvas.width, canvas.height);
          context.drawImage(img, 0, 0, canvas.width, canvas.height);
          const type = "image/png"
          // 图片转blob对象
          canvas.toBlob(blob => {
            const targetFile = new Blob([blob], {
              type
            })
            res(targetFile)
          }, type)
        })


      }


    </script>
  </body>


</html>
 ```

---



