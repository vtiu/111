<!--<template> 这个还不错
  <div class="video-upload">
    <div class="preview">
      <video v-if="videoUrl" :src="videoUrl" controls></video>
      <div v-else class="placeholder">视频预览</div>
    </div>
    <div class="upload-form">
      <input type="file" ref="fileInput" @change="handleFileChange">
      <button @click="submit" :disabled="!videoUrl">提交</button>
      <div v-if="errorMsg" class="error-msg">{{ errorMsg }}</div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        videoUrl: '',
        errorMsg: ''
      }
    },
    methods: {
      handleFileChange() {
        const file = this.$refs.fileInput.files[0]
        if (!file) return
        const reader = new FileReader()
        reader.readAsDataURL(file)
        reader.onload = () => {
          this.videoUrl = reader.result
        }
      },
      submit() {
        // 检查视频格式是否正确
        const file = this.$refs.fileInput.files[0]
        if (!file || !file.type.startsWith('video/')) {
          this.errorMsg = '请上传视频文件'
          return
        }
        // 提交视频
        // ...
        // 清空表单
        this.videoUrl = ''
        this.$refs.fileInput.value = ''
      }
    }
  }
</script>

<style>
  .video-upload {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1rem;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 0.5rem;
  }
  .preview {
    width: 100%;
    height: 20rem;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f0f0f0;
    border-radius: 0.5rem;
    overflow: hidden;
  }
  .placeholder {
    font-size: 2rem;
    color: #999;
  }
  .upload-form {
    display: flex;
    gap: 1rem;
    align-items: center;
  }
  button {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 0.25rem;
    background-color: #007aff;
    color: #fff;
    cursor: pointer;
  }
  button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  .error-msg {
    color: red;
  }
</style>-->

<!--在这个组件中，我们使用了`<input type="file">`元素来让用户选择要上传的视频文件。当用户选择文件后，我们通过`handleFileChange`方法来预览视频，并检查文件类型是否为视频。如果文件类型不是视频，我们会显示错误信息。

当用户点击上传按钮时，我们会模拟上传视频的过程，并将上传成功的视频信息添加到`uploadedVideos`数组中。同时，我们会清空文件选择框和预览框，并清除错误信息。

在模拟上传视频的过程中，我们使用了`setTimeout`函数来模拟上传的延迟。在实际应用中，我们需要使用真正的上传方法来上传视频。-->
<!--<template>
  <div>
    <h2>上传视频</h2>
    <div>
      <input type="file" ref="fileInput" @change="handleFileChange">
      <button @click="uploadVideo">上传</button>
    </div>
    <div v-if="previewUrl">
      <h3>预览</h3>
      <video :src="previewUrl" controls width="400"></video>
    </div>
    <div>
      <h3>已上传视频</h3>
      <ul>
        <li v-for="(video, index) in uploadedVideos" :key="index">
          <a :href="video.url" target="_blank">{{ video.name }}</a>
        </li>
      </ul>
    </div>
    <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        previewUrl: '',
        uploadedVideos: [],
        errorMessage: ''
      }
    },
    methods: {
      handleFileChange() {
        const file = this.$refs.fileInput.files[0]
        if (file) {
          if (file.type.startsWith('video/')) {
            this.previewUrl = URL.createObjectURL(file)
            this.errorMessage = ''
          } else {
            this.previewUrl = ''
            this.errorMessage = '请选择视频文件'
          }
        }
      },
      uploadVideo() {
        const file = this.$refs.fileInput.files[0]
        if (file) {
          if (file.type.startsWith('video/')) {
            // 模拟上传视频
            setTimeout(() => {
              this.uploadedVideos.push({
                name: file.name,
                url: 'http://example.com/' + file.name
              })
              this.$refs.fileInput.value = ''
              this.previewUrl = ''
              this.errorMessage = ''
            }, 1000)
          } else {
            this.errorMessage = '请选择视频文件'
          }
        } else {
          this.errorMessage = '请选择视频文件'
        }
      }
    }
  }
</script>

<style>
  .error-message {
    color: red;
  }
</style>-->



<template>
  <div>
    <div class="upload-box">
      <div class="preview">
        <div class="preview-box" v-if="previewUrl">
          <video :src="previewUrl" controls></video>
          <span class="close" @click="clearPreview">x</span>
        </div>
        <div class="preview-placeholder" v-else>
          <span class="icon"></span>
          <span class="text">视频预览区</span>
        </div>
      </div>
      <div class="upload-btn">
        <input type="file" ref="fileInput" @change="handleFileChange">
        <button @click="handleUpload">上传</button>
        <div v-if="error" class="error-message">{{ error }}</div>
      </div>
      <div v-if="errorCommit" class="error-message">{{ errorCommit }}</div>

    </div>
    <div class="upload-list">
      <div class="upload-item" v-for="(item, index) in uploadedList" :key="index">
        <div class="video-preview" @click="handlePreview(item.url)">
          <video :src="item.url"></video>
        </div>
        <div class="video-info">
          <span class="name">{{ item.name }}</span>
          <span class="size">{{ item.size }}MB</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        previewUrl: '',
        uploadedList: [],
        file: null,
        error: '',
        errorCommit:'',
      }
    },
    methods: {
      handleFileChange(e) {
        const file = e.target.files[0]
        if (!file) return
        if (!/video\/\w+/.test(file.type)) {
          this.error = '请上传视频文件'
          this.errorCommit=''
          return
        }
        if (file.size > 50 * 1024 * 1024) {
          this.error = '视频文件大小不能超过50MB'
          this.errorCommit=''
          return
        }
        this.file = file
        this.previewUrl = URL.createObjectURL(file)
        this.error = ''
        this.errorCommit=''
      },
      handleUpload() {
        if (!this.file) {
          this.errorCommit="请先选择文件"
          return
        }
        const formData = new FormData()
        formData.append('file', this.file)
        // 发送上传请求
        // axios.post('/upload', formData).then(res => {
        //   this.uploadedList.push({
        //     name: this.file.name,
        //     size: (this.file.size / 1024 / 1024).toFixed(2),
        //     url: res.data.url
        //   })
        // })
        this.uploadedList.push({
          name: this.file.name,
          size: (this.file.size / 1024 / 1024).toFixed(2),
          url: this.previewUrl
        })

        this.clearPreview()
      },

      handlePreview(url) {
        this.previewUrl = url
      },
      clearPreview() {
        this.previewUrl = ''
        this.file = null
        this.$refs.fileInput.value = ''
      }
    }
  }
</script>

<style scoped>
  .error-message {
    color: red;
  }
  .upload-box {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  .preview {
    position: relative;
    width: 300px;
    height: 200px;
    border: 1px solid #ccc;
    margin-right: 20px;
  }
  .preview-box {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
  .preview-box video {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .preview-placeholder {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    cursor: pointer;
  }
  .preview-placeholder .icon {
    width: 50px;
    height: 50px;
    background-color: #ccc;
    border-radius: 50%;
    margin-bottom: 10px;
  }
  .preview-placeholder .text {
    font-size: 14px;
    color: #999;
  }
  .close {
    position: absolute;
    top: 10px;
    right: 10px;
    width: 20px;
    height: 20px;
    line-height: 20px;
    text-align: center;
    background-color: rgba(0, 0, 0, 0.5);
    color: #fff;
    border-radius: 50%;
    cursor: pointer;
  }
  .upload-btn button {
    padding: 10px 20px;
    background-color: #409EFF;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  .upload-list {
    display: flex;
    flex-wrap: wrap;
  }
  .upload-item {
    width: 300px;
    margin-right: 20px;
    margin-bottom: 20px;
  }
  .upload-item:last-child {
    margin-right: 0;
  }
  .video-preview {
    position: relative;
    width: 100%;
    height: 200px;
    border: 1px solid #ccc;
    cursor: pointer;
  }
  .video-preview video {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .video-info {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 10px;
  }
  .video-info .name {
    font-size: 16px;
    color: #333;
  }
  .video-info .size {
    font-size: 14px;
    color: #999;
  }
</style>
