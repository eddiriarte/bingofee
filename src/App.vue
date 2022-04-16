<template>
  <div class="bingo">

    <section class="numbers-list">
      <div v-for="n in 90" :key="n" class="number-marker" :class="{ called: isCalled(n) }">{{n}}</div>
    </section>

    <button @click="pickNumber" role="button">Ziehen!</button>
    <hr>
    <div v-for="n in called" :key="n" class="called-number">{{n}}</div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      called: [],
    }
  },
  methods: {
    isCalled(number) {
      return this.called.includes(number)
    },
    pickNumber() {
      const n = Math.floor(Math.random() * 89) + 1;

      if (this.called.includes(n)) {
        return this.pickNumber()
      }

      this.called.push(n)

      const msg = new SpeechSynthesisUtterance()
      msg.text = `Nummer ${n}`
      msg.lang = 'de-DE'
      msg.voice = this.getVoice()
      
      window.speechSynthesis.speak(msg)
    },
    getVoice() {
      const synth = window.speechSynthesis;
      return synth.getVoices().filter(v => v.lang == 'de-DE')[0]
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.numbers-list {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  max-width: 55%;
  margin: auto;
}

.number-marker {
  width: 40px;
  height: 40px;
  background-color: #74b9ff;
  border-radius: 50%;
  border: 2px solid #0984e3;
  margin: 4px auto;
  color: #333;
  font-weight: bold;
  font-size: 28px;
}

.number-marker.called {
  background-color: #fab1a0;
  border: 2px solid #d63031;
  color: #d63031;
}
.called-number {
  display: inline-grid;
  flex: row nowrap;
  margin: auto 8px;
}
</style>
