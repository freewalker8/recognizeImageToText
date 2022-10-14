<template>
  <div class="container">
    <h3>图片文字识别</h3>
    <div style="margin: 20px 0">
      <input type="file" name="imgFile" id="imgFile" />
      <input type="button" @click="recognizeImage" value="识别文字" />
    </div>
    <div class="readResult">
      <h5>{{ fileName }}&nbsp;识别结果：<span v-if="reading">读取中，请稍等...</span></h5>
      <div class="text">
        <textarea v-model="recognizeResult"></textarea>
      </div>
      <div class="img" v-if="imageUrl">
        <p>源图片</p>
        <img :src="imageUrl" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageToText',
  data() {
    return {
      files: '',
      fileName: '',
      recognizeResult: '',
      imageUrl: '',
      reading: false
    }
  },
  methods: {
    recognizeImage() {
      const that = this;
      const files = document.getElementById('imgFile');
      const file = files.files[0];
      const fileName = file.name;
      if (!['.bmp', '.jpg', '.jpeg', '.png', '.pbm'].some(ty => {
        return fileName.endsWith(ty);
      })) {
        alert("文件格式不正确，支持'.bmp', '.jpg', '.jpeg', '.png', '.pbm'");
        return;
      }
      const fileReader = new FileReader();
      fileReader.readAsDataURL(file);
      fileReader.onload = function() {
        const objUrl = this.result;
        that.imageUrl = objUrl;
      }
      this.fileName = fileName;
      this.reading = true;
      this.recognizeResult = '';
      try {
        window.Tesseract.recognize(
          file,
          'chi_sim',
          {
            langPath: '/lang',
            // langPath: '/lang/fast',
            // corePath: '/tesseract-core.wasm.js',
            logger: m => console.log(m)
          }
        ).then(({ data: { text } }) => {
          console.log('识别结果：', text);
          this.reading = false;
          this.recognizeResult = text;
          document.getElementById('imgFile').value = null; // clear
        })			
      } catch (error) {
        this.reading = false;
        alert('识别出错，请再试一次');
      }
    }
  }
}
</script>

<style scoped>
.container {
  padding: 20px;
  width: 1400px;
  height: calc(100% - 40px);
  margin: 0 auto;
  background-color: #f5f5f5;
}
.readResult {
  height: calc(100% - 150px);
  margin: 0 0 20px 0;
  padding: 20px;
  background-color: #fff;
}
.readResult h5 {
  margin: 0 0 20px;
}
.readResult .text {
  width: 50%;
  height: 90%;
  display: block;
  float: left;
  padding-right: 10px;
}
.readResult .text textarea {
  width: 100%;
  height: 100%;
  display: block;
}
.readResult .img {
  width: calc(50% - 20px);
  height: 100%;
  padding-left: 10px;
  float: right;
}
.readResult .img img {
  width: 100%;
}
</style>
