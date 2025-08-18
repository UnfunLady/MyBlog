---
title: "webview在小程序预览文件"
catalog: true
date: 2025-04-17 17:46:57
subtitle: "小程序预览文件"
header-img:
tags:
  - webview
  - 微信小程序
  - JavaScript
catagories:
  - JavaScript
---

### 笔记描述

> 用于 webview 在小程序预览文件

### 主要代码

```js
try {
  const ua = navigator.userAgent.toLowerCase();
  if (ua.match(/MicroMessenger/i) == "micromessenger") {
    //ios的ua中无miniProgram，但都有MicroMessenger（表示是微信浏览器）
    wx.miniProgram.getEnv((res) => {
      if (res.miniprogram) {
        //在微信内，在小程序内。
        if ($global.downLoadType == 1) {
          $global.showDownLoad = false;
          $Toast.show({
            content: "长按证书图片即可保存",
          });
          return;
        }
        const id = $history?.location?.query?.id;
        let url = `/api/exam/certificate/export?id=${id}&fileType=${$global.downLoadType}`;
        wx.miniProgram.navigateTo({
          url: `/pages/file-download/file-download?url=${encodeURIComponent(
            url
          )}&isImg=${$global.downLoadType == 1 ? 1 : 0}`,
        });
        $global.showDownLoad = false;
        return;
      } else {
        //在微信内，不在小程序内。
        $global.showDownLoad = false;
        $Toast.show({
          content: "长按证书图片即可保存",
        });
      }
    });
  } else {
    //不在微信内。
    $props({
      loading: true,
    });
    $global.showDownLoad = false;
    const id = $history?.location?.query?.id;
    $fetch("/api/exam/certificate/export", {
      method: "get",
      data: {},
      getResponse: true,
      responseType: "blob",
      params: {
        id: id,
        //默认下载pdf
        fileType: $global.downLoadType,
      },
    })
      .then((res) => {
        $props({
          loading: false,
        });
        const url = window.URL.createObjectURL(new Blob([res.data]));
        const a = document.createElement("a");
        if (res.response) {
          console.log(res.response);
          const fileNameMatch = res.response?.headers
            ?.get("Content-disposition")
            ?.match(/=(.*)$/)[1];
          fileName = unescape(decodeURI(fileNameMatch || "")).replace(
            /\+/g,
            " "
          ); //解码并替换加号为空格
          fileName = decodeURI(fileName);
          a.download = fileName;
          a.href = url;
          a.click();
          window.URL.revokeObjectURL(url);
        } else {
          $message.warn("下载失败");
        }
      })
      .catch((e) => {
        console.log(e);
        $props({
          loading: false,
        });
        $message.warn("下载失败");
      })
      .finally(() => {
        $props({
          loading: false,
        });
      });
  }
} catch (error) {
  console.error("error:", error);
}
```

```JS
	onLoad(e) {
		console.log(e);
		if (e.url) {
			const url = decodeURIComponent(e.url);
			const isImg = e.isImg == 1 ? true : false;
			uni.showLoading({
				title: '正在下载...',
				mask: true
			});
			uni.downloadFile({
				url: conf.serverUrl + url,
				header: {
					Authorization: 'Bearer ' + this.$store.state.userInfo.access_token
				},
				success: (res) => {
					if (res.statusCode == 200) {
						uni.hideLoading();
						uni.showModal({
							title: '提示',
							content: '文件下载成功 请自行保存文件',
							success: (r) => {
								if (r.confirm) {
									uni.showLoading({
										title: '正在打开文件...',
										mask: true
									});
									const contentDisposition = res.header['Content-disposition'];
									let fileType = '';
									if (contentDisposition && contentDisposition.indexOf('filename=') !== -1) {
										fileType = decodeURIComponent(contentDisposition.split('filename=')[1]).split('.')[1];
									}
									wx.openDocument({
										filePath: res.tempFilePath,
										fileType: fileType || 'pdf',
										showMenu: true,
										success: () => {
											uni.showToast({
												title: '文件打开成功 请保存文件',
												icon: 'none'
											});
											setTimeout(() => {
												uni.navigateBack();
											}, 1500);
										},
										fail: (e) => {
											console.log(e);
											uni.showToast({
												title: '打开文件失败',
												icon: 'none'
											});
											setTimeout(() => {
												uni.navigateBack();
											}, 1500);
										}
									});
								} else if (res.cancel) {
									uni.navigateBack();
								}
							}
						});
					} else {
						uni.showToast({
							title: '文件下载失败',
							icon: 'none'
						});
						setTimeout(() => {
							uni.navigateBack();
						}, 1500);
					}
				},
				fail: (error) => {
					console.log(error);
					uni.showToast({
						title: '文件下载失败',
						icon: 'none'
					});
					setTimeout(() => {
						uni.navigateBack();
					}, 1500);
				}
			});
		}
	}
```
