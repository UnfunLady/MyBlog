---
title: "文件hash计算 不入库原件"
catalog: true
date: 2023-11-13 22:41:19
subtitle: "计算文件hash"
header-img:
tags:
  - 文件存储
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

`文件 hash 计算不入库原件 对比 hash 确定是否修改了文件值 `

### 主要代码

```js
// 引入依赖
<script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.0.0/crypto-js.min.js"></script>
```

```html
<body>
  <input type="file" id="fileInput" />
  <script>
    document
      .getElementById("fileInput")
      .addEventListener("change", function (event) {
        const file = event.target.files[0];
        const fileSize = file.size;
        const chunkSize = 1024 * 1024; // 1MB per chunk
        let offset = 0;
        let hashes = [];

        const reader = new FileReader();
        reader.onload = function (e) {
          const hash = CryptoJS.SHA256(e.target.result);
          hashes.push(hash.toString());
          offset += e.target.result.byteLength;
          if (offset < fileSize) {
            readNextChunk();
          } else {
            // Combine hashes (optional)
            const finalHash = CryptoJS.SHA256(hashes.join(""));
            // 获取hash入库
            console.log("Final Hash:", finalHash.toString());
          }
        };
        reader.onerror = function (e) {
          console.error("Error reading file", e);
        };

        function readNextChunk() {
          const chunk = file.slice(offset, offset + chunkSize);
          reader.readAsArrayBuffer(chunk);
        }
        readNextChunk(); // Start reading the first chunk
      });
  </script>
</body>
```
