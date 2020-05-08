<template>
  <view class="compressImage">
    <canvas :style="{ 'width': cWidth + 'px', 'height': cHeight + 'px', 'position': 'absolute', 'z-index': -1, 'left': '-10000rpx', 'top': '-10000rpx' }" canvas-id="canvas"></canvas>
    <button @getuserinfo="clickButton" data-number="0" open-type="getUserInfo">点击按钮</button>
  </view>
</template>

<script>
  import utils from '@/utils/utils.js';
  export default {
    data() {
      return {
        cWidth: 480, // 最大宽
        cHeight: 640, // 最大高
      }
    },
    onLoad() {

    },
    methods: {
      async clickButton(e) { // 点击按钮
        const image = await this.cameraPhoto(e); // 选择图片
        if (!image) {
          return;
        }
        
        const result = await utils.compressionIamge(e, this, image); // 压缩图片 
        console.log(result);
        if (!result) {
          return;
        }

        uni.saveImageToPhotosAlbum({ // 导出图片到本地，以便查看压缩大小是否正确。
          filePath: result,
          success: () => {
            console.log('save success');
          },
          fail: (err) => {
            console.log(err);
          }
        });
      },
      
      async cameraPhoto() { // 选择图片
        const result = await uni.chooseImage({
          count: 1,
          sizeType: ['original'],
          sourceType: ['camera', 'album']
        });
        if (result[0] !== null) {
          return;
        }
        return result[1]['tempFilePaths'][0];
        // try catch
      }
    }
  }
</script>

<style>

</style>
