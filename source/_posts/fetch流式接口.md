---
title: "fetch流式接口"
catalog: true
date: 2025-03-24 19:31:02
subtitle: "流式接口数据"
header-img:
tags:
  - fetch
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于 AI 对话相关获取 AI 数据输出

### 主要代码

```js
try {
  const submit_form = {
    question: userInput.value,
    session_id: $global.sessionId || "",
  };
  const chatResponse = await fetch(
    window.location.origin + `/agent/api/v1/datasets/ask`,
    {
      method: "POST",
      headers: {
        Authorization: "Bearer " + localStorage.getItem("access_token"),
        "content-Type": "application/json",
      },
      body: JSON.stringify(submit_form),
    }
  );
  await streamResponse(chatResponse);
} catch (error) {
  $global.inResponse = false;
  updateLastMessage("抱歉，发生了错误：" + error.message);
}

const streamResponse = async (response) => {
  const reader = response.body.getReader();
  const decoder = new TextDecoder();
  let partialMessage = "";
  let webSearchResults = [];
  let retrieverResources = [];
  let buffer = "";
  try {
    while (true) {
      const { value, done } = await reader.read();
      if (done) {
        $global.inResponse = false;
        break;
      }
      const text = decoder.decode(value);
      buffer += text;
      const lines = buffer.split("\n");
      buffer = lines.pop() || "";
      for (const line of lines) {
        if (line.startsWith("data:")) {
          const jsonStr = line.slice(5);
          if (jsonStr.trim() === "[DONE]") continue;
          try {
            // do
          } catch (e) {
            console.error("JSON parse error:", e, "for line:", jsonStr);
          }
        }
      }
    }
  } catch (error) {
    console.error("Stream reading error:", error);
  }
};
```
