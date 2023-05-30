<!--<template>
  <div class="metronome">
    <div class="controls">
      <button @click="toggleMetronome">{{ isPlaying ? '暂停' : '开始' }}</button>
      <button @click="resetMetronome">重置</button>
      <div class="tempo">
        <label>节拍速度: {{ tempo }} BPM</label>
        <input type="range" min="40" max="240" v-model="tempo" />
      </div>
      <div class="beats">
        <label>每拍节拍数: {{ beats }}</label>
        <input type="range" min="1" max="8" v-model="beats" />
      </div>
    </div>
    <div class="metronome-display">
      <div class="beat" :class="{ accent: isAccent }"></div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        isPlaying: false,
        tempo: 120,
        beats: 4,
        currentBeat: 1,
        isAccent: false,
        audioContext: null,
        oscillator: null,
        gainNode: null,
        intervalId: null,
      };
    },
    methods: {
      toggleMetronome() {
        if (this.isPlaying) {
          this.stopMetronome();
        } else {
          this.startMetronome();
        }
        this.isPlaying = !this.isPlaying;
      },
      startMetronome() {
        this.audioContext = new AudioContext();
        this.oscillator = this.audioContext.createOscillator();
        this.gainNode = this.audioContext.createGain();
        this.oscillator.connect(this.gainNode);
        this.gainNode.connect(this.audioContext.destination);
        this.oscillator.type = 'triangle';
        this.oscillator.frequency.value = 440;
        this.gainNode.gain.value = 0;
        this.oscillator.start(0);
        this.intervalId = setInterval(this.playBeat, this.getInterval());
      },
      stopMetronome() {
        clearInterval(this.intervalId);
        this.audioContext.close();
        this.oscillator = null;
        this.gainNode = null;
        this.audioContext = null;
        this.currentBeat = 1;
        this.isAccent = false;
      },
      resetMetronome() {
        if (this.isPlaying) {
          this.stopMetronome();
        }
        this.tempo = 120;
        this.beats = 4;
      },
      playBeat() {
        const beatDuration = 60 / this.tempo;
        const accentBeat = this.beats === 4 ? 1 : 2;
        if (this.currentBeat === accentBeat) {
          this.isAccent = true;
          this.gainNode.gain.value = 1;
        } else {
          this.isAccent = false;
          this.gainNode.gain.value = 0.5;
        }
        setTimeout(() => {
          this.gainNode.gain.value = 0;
        }, 100);
        this.currentBeat = this.currentBeat === this.beats ? 1 : this.currentBeat + 1;
      },
      getInterval() {
        const beatDuration = 60 / this.tempo;
        return beatDuration * 1000;
      },
    },
  };
</script>

<style>
  .metronome {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background-color: #f5f5f5;
  }

  .controls {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-bottom: 20px;
  }

  .controls button {
    margin: 5px;
    padding: 10px;
    border-radius: 5px;
    background-color: #4caf50;
    color: #fff;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.2s ease-in-out;
  }

  .controls button:hover {
    background-color: #3e8e41;
  }

  .tempo,
  .beats {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 10px;
  }

  .tempo input,
  .beats input {
    width: 200px;
  }

  .metronome-display {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 200px;
    width: 200px;
    border-radius: 50%;
    background-color: #fff;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    position: relative;
  }

  .beat {
    height: 100px;
    width: 10px;
    background-color: #333;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    animation: beat-animation 1s ease-in-out infinite;
  }

  .beat.accent {
    height: 120px;
    background-color: #f44336;
  }

  @keyframes beat-animation {
    0% {
      height: 100px;
    }
    50% {
      height: 120px;
    }
    100% {
      height: 100px;
    }
  }
</style>-->

<!--<template>
  <div id="app">
    <div class="container">
      <div class="title">专业节拍器</div>
      <div class="beat-container">
        <div class="beat" :class="{ active: isActive }" @click="toggleActive"></div>
        <div class="beat" :class="{ active: isActive }" @click="toggleActive"></div>
        <div class="beat" :class="{ active: isActive }" @click="toggleActive"></div>
        <div class="beat" :class="{ active: isActive }" @click="toggleActive"></div>
      </div>
      <div class="tempo-container">
        <div class="tempo-label">BPM</div>
        <div class="tempo-input">
          <input type="number" v-model="tempo" min="1" max="300" @input="updateTempo" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>

    export default {
      data() {
        return {
          isActive: false,
          tempo: 120,
          audio: null,
          intervalId: null,
        };
      },
      mounted() {
        this.audio = new Audio("https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3");
        this.audio.loop = true;
      },
      methods: {
        toggleActive() {
          this.isActive = !this.isActive;
          if (this.isActive) {
            this.startInterval();
            this.audio.play();
          } else {
            this.stopInterval();
            this.audio.pause();
          }
        },
        startInterval() {
          const interval = 60 / this.tempo * 1000;
          this.intervalId = setInterval(() => {
            this.audio.currentTime = 0;
            this.audio.play();
          }, interval);
        },
        stopInterval() {
          clearInterval(this.intervalId);
        },
        updateTempo() {
          if (this.isActive) {
            this.stopInterval();
            this.startInterval();
          }
        },


    },
  };
</script>

<style>
  body {
    background-color: #f2f2f2;
    font-family: Arial, sans-serif;
  }

  .container {
    max-width: 600px;
    margin: 0 auto;
    text-align: center;
  }

  .title {
    font-size: 36px;
    font-weight: bold;
    margin-bottom: 30px;
  }

  .beat-container {
    display: flex;
    justify-content: center;
    margin-bottom: 30px;
  }

  .beat {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background-color: #fff;
    margin: 0 10px;
    cursor: pointer;
    transition: all 0.2s ease-in-out;
  }

  .beat.active {
    background-color: #ff6b6b;
    box-shadow: 0 0 10px #ff6b6b;
  }

  .tempo-container {
    display: flex;
    justify-content: center;
  }

  .tempo-label {
    font-size: 24px;
    font-weight: bold;
    margin-right: 10px;
  }

  .tempo-input {
    width: 100px;
    height: 50px;
    border: none;
    border-radius: 5px;
    background-color: #fff;
    text-align: center;
    font-size: 24px;
    font-weight: bold;
    color: #333;
  }
</style>-->

<template>
  <div class="metronome">
    <div class="metronome__display">{{ bpm }} BPM</div>
    <div class="metronome__controls">
      <button class="metronome__button" @click="decreaseBpm">-</button>
      <button class="metronome__button" @click="togglePlaying">
        {{ playing ? 'Stop' : 'Start' }}
      </button>
      <button class="metronome__button" @click="increaseBpm">+</button>
    </div>
    <div class="metronome__animation" :class="{ 'metronome__animation--playing': playing }">
      <div class="metronome__animation__bar"></div>
      <div class="metronome__animation__bar"></div>
      <div class="metronome__animation__bar"></div>
      <div class="metronome__animation__bar"></div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        bpm: 120,
        playing: false,
        audio: new Audio(require('D:/Download/stance.mp3')),
        intervalId: null,
      };
    },
    methods: {
      togglePlaying() {
        if (this.playing) {
          clearInterval(this.intervalId);
        } else {
          this.intervalId = setInterval(() => {
            this.audio.currentTime = 0;
            this.audio.play();
          }, 60000 / this.bpm);
        }
        this.playing = !this.playing;
      },
      increaseBpm() {
        this.bpm += 5;
      },
      decreaseBpm() {
        this.bpm -= 5;
      },
    },
  };
</script>

<style scoped>
  .metronome {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }

  .metronome__display {
    font-size: 5rem;
    margin-bottom: 2rem;
  }

  .metronome__controls {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 2rem;
  }

  .metronome__button {
    font-size: 2rem;
    margin: 0 1rem;
    padding: 1rem 2rem;
    border: none;
    border-radius: 0.5rem;
    background-color: #333;
    color: #fff;
    cursor: pointer;
    transition: background-color 0.2s ease;
  }

  .metronome__button:hover {
    background-color: #666;
  }

  .metronome__animation {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 10rem;
    width: 100%;
    background-color: #fff;
    transition: background-color 0.2s ease;
  }

  .metronome__animation--playing {
    background-color: #333;
  }

  .metronome__animation__bar {
    height: 6rem;
    width: 1rem;
    margin: 0 0.5rem;
    background-color: #333;
    animation: pulse 1s ease infinite;
  }

  .metronome__animation__bar:nth-child(1) {
    animation-delay: 0s;
  }

  .metronome__animation__bar:nth-child(2) {
    animation-delay: 0.25s;
  }

  .metronome__animation__bar:nth-child(3) {
    animation-delay: 0.5s;
  }

  .metronome__animation__bar:nth-child(4) {
    animation-delay: 0.75s;
  }

  @keyframes pulse {
    0% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.2);
    }
    100% {
      transform: scale(1);
    }
  }
</style>
