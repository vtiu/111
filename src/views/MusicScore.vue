<!--
<template>
  <v-container>
    <v-row>
      <v-col cols="12" sm="6">
        <v-text-field label="曲谱名称" v-model="name"></v-text-field>
      </v-col>
      <v-col cols="12" sm="6">
        <v-select label="调号" v-model="key" :items="keys"></v-select>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" sm="6">
        <v-text-field label="节拍" v-model="time"></v-text-field>
      </v-col>
      <v-col cols="12" sm="6">
        <v-select label="节拍类型" v-model="timeType" :items="timeTypes"></v-select>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-textarea label="曲谱内容" v-model="content"></v-textarea>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12">
        <v-btn color="primary" @click="preview">预览</v-btn>
        <v-btn color="primary" @click="play">播放</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import VueMidi from 'vue-midi'
  import Vuetify from 'vuetify'

  export default {
    name: 'MusicEditor',
    components: {
      VueMidi,
      Vuetify
    },
    data () {
      return {
        name: '',
        key: '',
        keys: ['C', 'D', 'E', 'F', 'G', 'A', 'B'],
        time: '',
        timeType: '',
        timeTypes: ['2/4', '3/4', '4/4', '6/8', '9/8', '12/8'],
        content: ''
      }
    },
    methods: {
      preview () {
        // 将曲谱转换为MIDI文件
        const midi = this.$midi.fromMusicXML(this.content)

        // 将MIDI文件转换为Blob对象
        const blob = new Blob([midi], { type: 'audio/midi' })

        // 将Blob对象转换为URL
        const url = URL.createObjectURL(blob)

        // 创建一个<audio>元素并播放MIDI文件
        const audio = new Audio(url)
        audio.play()
      },
      play () {
        // 播放曲谱
        this.$midi.playMusicXML(this.content)
      }
    }
  }
</script>

<style>
  .music-editor {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100%;
  }

  @media (min-width: 600px) {
    .music-editor {
      flex-direction: row;
    }
  }
</style>
-->
