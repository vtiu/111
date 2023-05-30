<template>
  <div class="piano">
    <div class="keys">
      <div class="key white" v-for="(note, index) in notes" :key="index" :class="{ active: activeNote === note }" @mousedown="playNote(note)" @mouseup="stopNote()" @mouseleave="stopNote()"></div>
      <div class="key black" :style="{ visibility: index === 2 ? 'hidden' : 'block' }" v-for="(note, index) in blackNotes" :key="index" :class="{ active: activeNote === note }" @mousedown="playNote(note)" @mouseup="stopNote()" @mouseleave="stopNote()">

      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        notes: ['C', 'D', 'E', 'F', 'G', 'A', 'B', 'C2'],
        blackNotes: ['C#', 'D#', '', 'F#', 'G#', 'A#'],
        activeNote: null,
        audioContext: null,
        oscillator: null,
        gainNode: null
      }
    },
    mounted() {
      this.audioContext = new AudioContext()
      this.gainNode = this.audioContext.createGain()
      this.gainNode.connect(this.audioContext.destination)
    },
    methods: {
      playNote(note) {
        this.activeNote = note
        const frequency = this.getFrequency(note)
        this.oscillator = this.audioContext.createOscillator()
        this.oscillator.type = 'sine'
        this.oscillator.frequency.setValueAtTime(frequency, this.audioContext.currentTime)
        this.oscillator.connect(this.gainNode)
        this.oscillator.start()
      },
      stopNote() {
        this.activeNote = null
        this.oscillator.stop()
        this.oscillator.disconnect()
      },
      getFrequency(note) {
        const notes = {
          'C': 261.63,
          'C#': 277.18,
          'D': 293.66,
          'D#': 311.13,
          'E': 329.63,
          'F': 349.23,
          'F#': 369.99,
          'G': 392.00,
          'G#': 415.30,
          'A': 440.00,
          'A#': 466.16,
          'B': 493.88,
          'C2': 523.25
        }
        return notes[note]
      }
    }
  }
</script>

<style>
  . black[blackNotes=""] {
    display: none;
  }
  .piano {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 300px;
  }

  .keys {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
  }

  .key {
    position: relative;
    width: 40px;
    height: 200px;
    margin: 0 5px;
    border-radius: 5px;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
    cursor: pointer;
  }

  .white {
    background-color: #fff;
  }

  .black {
    position: relative;
    top: 0;
    left: -374px;
    width: 39px;
    height: 120px;
    background-color: #000;
    border-radius: 5px 5px 0 0;


  }


  .black:nth-child(1) {
    top: 0;
    left: 35px;
  }

  .black:nth-child(2) {
    top: 0;
    left: 85px;
  }

  .black:nth-child(3) {
    top: 0;
    left: 175px;

  }

  .black:nth-child(4) {
    top: 0;
    left: 225px;
  }

  .black:nth-child(5) {
    top: 0;
    left: 275px;
  }

  .black:nth-child(6) {
    top: 0;
    left: 365px;
  }

  .active {
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  }
</style>
