
# sixteen-compression-iamge
uni-app 压缩图片 等比例压缩图片 可指定图片宽高

## 源码 utils.js
```javascript
export default {
  compressionIamge(e, that, image) { // 等比例压缩图片 可指定图片宽高 兼容性：微信小程序端正常，其他端未测试
    uni.showLoading({
      title: "正在压缩图片"
    });
    let result = null;
    const promise = new Promise((resolve, reject) => {
      uni.getImageInfo({
        src: image,
        success: (res) => {
          let originHeight = res.height;
          let originWidth = res.width;
          let maxWidth = 480; // 最大宽
          let maxHeight = 640; // 最大高
          let targetWidth = originWidth
          let targetHeight = originHeight;
          if (originWidth > maxWidth || originHeight > maxHeight) {
            if (originWidth / originHeight > maxWidth / maxHeight) {
              targetWidth = maxWidth;
              targetHeight = Math.round(maxWidth * (originHeight / originWidth));
            } else {
              targetHeight = maxHeight;
              targetWidth = Math.round(maxHeight * (originWidth / originHeight));
            }
          }
          
          that.cWidth = targetWidth;
          that.cHeight = targetHeight

          let ctx = uni.createCanvasContext('canvas', that);
          ctx.clearRect(0, 0, targetWidth, targetHeight);
          ctx.drawImage(image, 0, 0, targetWidth, targetHeight);
          ctx.draw(false, setTimeout(() => {
            uni.canvasToTempFilePath({
              canvasId: 'canvas',
              success: (res) => {
                resolve(res.tempFilePath);
              },
              fail: (err) => {
                console.log(err);
              },
              complete: () => {
                uni.hideLoading();
              }
            }, that);
          }, 1000));
        }
      })
    });
    return promise.then(res => result = res);
  }
}

```

## 调用 index.vue

```html
canvas标签一定要写，剩下的可以自己根据案例修改，不懂的可以问我 qweabc1024@163.com
```

```javascript
const result = await utils.compressionIamge(e, this, image); // 压缩图片 
```
## 兼容性
只在微信小程序测试可用，其他端未知。


谢谢！

https://github.com/Liuxianlu/compressionIamge
