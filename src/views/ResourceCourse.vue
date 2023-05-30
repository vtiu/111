<template>
  <div>
    <h1>视频列表</h1>
    <div v-for="video in videos" :key="video.id">
      <img :src="video.cover" alt="视频封面">
      <video :src="video.url" controls></video>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    data() {
      return {
        videos: [],
      };
    },
    mounted() {
      this.getVideos();
    },
    methods: {
      async getVideos() {
        try {
          const response = await axios.get('/api/videos');
          this.videos = response.data;
        } catch (error) {
          console.error(error);
        }
      },
    },
  };
</script>
