<!--
<template>
  <div>
    <h1>调音器</h1>
    <div>
      <label>频率</label>
      <input type="range" min="20" max="20000" v-model="frequency" @input="setFrequency">
    </div>
    <div>
      <label>音量</label>
      <input type="range" min="0" max="1" step="0.01" v-model="volume" @input="setVolume">
    </div>
    <button @click="start">开始</button>
    <button @click="stop">停止</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        audioContext: new (window.AudioContext || window.webkitAudioContext)(),
        oscillator: null,
        gainNode: null,
        frequency: 440,
        volume: 0.5
      }
    },
    methods: {
      start() {
        this.oscillator = this.audioContext.createOscillator();
        this.gainNode = this.audioContext.createGain();
        this.oscillator.connect(this.gainNode);
        this.gainNode.connect(this.audioContext.destination);
        this.oscillator.start();
      },
      stop() {
        this.oscillator.stop();
      },
      setFrequency() {
        this.oscillator.frequency.setValueAtTime(this.frequency, this.audioContext.currentTime);
      },
      setVolume() {
        this.gainNode.gain.setValueAtTime(this.volume, this.audioContext.currentTime);
      }
    }
  }
</script>

<style>
  body {
    background: linear-gradient(to bottom, #ff8a00, #da1b60);
    font-family: 'Helvetica Neue', sans-serif;
    color: #fff;
    text-align: center;
  }
  input[type="range"] {
    width: 80%;
    margin: 0 auto;
    -webkit-appearance: none;
    background-color: #fff;
    height: 10px;
    border-radius: 5px;
  }
  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0 0 2px rgba(0, 0, 0, 0.5);
  }
  button {
    margin-top: 20px;
    background-color: #fff;
    color: #da1b60;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  button:hover {
    background-color: #da1b60;
    color: #fff;
  }
</style>-->

<template>
  <div>
    <div class="tuner">
      <div class="tuner-display">{{ frequency }} Hz</div>
      <div class="tuner-dial" :style="{ transform: 'rotate(' + dialAngle + 'deg)' }" @mousedown="isDragging = true" @mousemove="isDragging && setFrequency(getFrequency($event))" @mouseup="isDragging = false" @mouseleave="isDragging = false">
        <div class="tuner-pointer"></div>
      </div>
      <div class="tuner-controls">
        <button class="tuner-button" @click="decreaseFrequency" type="range" min="20" max="20000" v-model="frequency">-</button>
        <button class="tuner-button" @click="increaseFrequency" type="range" min="20" max="20000" v-model="frequency">+</button>
      </div>

    </div>
    <button @click="start">开始</button>
    <button @click="stop">停止</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        audioContext: null,
        oscillator: null,
        gainNode: null,
        volume: 0.5,
        frequency: 440,
        dialAngle: 0
      };
    },
/*    mounted() {
      this.audioContext = new (window.AudioContext || window.webkitAudioContext)()
      this.oscillator = this.audioContext.createOscillator()
      this.gainNode = this.audioContext.createGain()
      this.oscillator.connect(this.gainNode)
      this.gainNode.connect(this.audioContext.destination)
      this.oscillator.start(0)
    },*/
    methods: {
      start() {
        this.audioContext = new (window.AudioContext || window.webkitAudioContext)()
        this.oscillator = this.audioContext.createOscillator()
        this.gainNode = this.audioContext.createGain()
        this.oscillator.connect(this.gainNode)
        this.gainNode.connect(this.audioContext.destination)
        this.oscillator.start(0)
      },
      stop() {
        this.oscillator.stop();
      },
      setFrequency(frequency) {
        this.oscillator.frequency.setValueAtTime(frequency, this.audioContext.currentTime)
        this.frequency = frequency
      },
/*      setVolume(volume) {
        this.gainNode.gain.setValueAtTime(volume, this.audioContext.currentTime)
        this.volume = volume
      },*/

      increaseFrequency() {
        this.frequency += 1;
        this.oscillator.frequency.setValueAtTime(this.frequency, this.audioContext.currentTime);
        this.updateDialAngle();
      },
      decreaseFrequency() {
        this.frequency -= 1;
        this.oscillator.frequency.setValueAtTime(this.frequency, this.audioContext.currentTime);
        this.updateDialAngle();
      },
      updateDialAngle() {
        this.dialAngle = (this.frequency - 440) * 0.5;
      }
    }
  };
</script>

<style>
  .tuner {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 300px;
    width: 300px;
    background-color: #333;
    border-radius: 50%;
    position: relative;
  }

  .tuner-display {
    font-size: 3rem;
    color: #fff;
    margin-bottom: 1rem;
  }

  .tuner-dial {
    height: 200px;
    width: 200px;
    border-radius: 50%;
    background-color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
  }

  .tuner-pointer {
    height: 100px;
    width: 10px;
    background-color: #f00;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    border-radius: 5px;
  }

  .tuner-controls {
    display: flex;
    justify-content: center;
    position: absolute;
    bottom: 0;
    width: 100%;
  }

  .tuner-button {
    height: 50px;
    width: 50px;
    border-radius: 50%;
    background-color: #fff;
    color: #333;
    font-size: 2rem;
    margin: 0 1rem;
    cursor: pointer;
    border: none;
    outline: none;
    transition: all 0.2s ease-in-out;
  }

  .tuner-button:hover {
    background-color: #333;
    color: #fff;
  }
</style>
