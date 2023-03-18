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
    <input type="file" id="ip">
    <script>
        ip.onchange = async function (e) {
            // 原图尺寸
            console.log((e.target.files[0].size / 1024 / 1024) > 1 ? (e.target.files[0].size / 1024 / 1024).toFixed(2) + "mb" : ((e.target.files[0].size / 1024 / 1024) * 1000).toFixed(2) + "kb");
            const i = await imgCompress(e.target.files[0])
            // 压缩后尺寸
            console.log((i.size / 1024 / 1024) > 1 ? (i.size / 1024 / 1024).toFixed(2) + "mb" : ((i.size / 1024 / 1024) * 1000).toFixed(2) + "kb");
            const img = document.createElement("img")
            img.src = URL.createObjectURL(i)
            document.body.append(img)
        }

        function imgToFile(f) {
            let img = new Image()
            let fileReader = new FileReader()
            return new Promise((res, rej) => {
                fileReader.readAsDataURL(f)
                img.onload = function () {
                    res(img)
                }
                fileReader.onload = function () {
                    img.src = fileReader.result
                }
            })
        }

        async function imgCompress(i) {
            const canvas = document.createElement("canvas")
            const img = await imgToFile(i)
            const ctx = canvas.getContext("2d")
            return new Promise((res, rej) => {
                const { width: originWidth, height: originHeight } = img
                const targetWidth = 300
                const scale = (originWidth / originHeight)
                const targetHeight = Math.floor((targetWidth / scale))
                // 判断压缩后的目标大小是否比原图大 如果是则用原图 否则用压缩后的大小
                if (targetWidth > originWidth) {
                    canvas.width = originWidth;
                    canvas.height = targetHeight > originHeight ? originHeight : targetHeight

                } else {
                    canvas.width = targetWidth;
                    canvas.height = targetHeight > originHeight ? originHeight : targetHeight
                }
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                ctx.drawImage(img, 0, 0, canvas.width, canvas.height)
                const type = "image/png";
                canvas.toBlob((blob) => {
                    const f = new Blob([blob], { type })
                    res(f)
                })
            })
        }
    </script>
</body>

</html>
 ```

---



