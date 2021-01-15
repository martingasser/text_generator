<template>
  <h1>Enter a seed phrase</h1>
  <input type="text" v-model="inputText">
  <button @click="generate">Generate</button>
  <h1>GPT-2 continuation</h1>
  <h2 ref="waiting"></h2>
  <div id="output" ref="output">{{ outputText }}</div>
</template>

<script>
import deepai from 'deepai'

export default {
  name: 'App',
  data () {
    return {
      inputText: '',
      outputText: ''
    }
  },
  methods: {
    startWaiting () {
      this.ctr = 0
      this.interval = setInterval(() => {
        if (this.ctr % 2 === 0) {
          this.$refs.waiting.innerHTML = 'Please'
        } else {
          this.$refs.waiting.innerHTML = 'wait'
        }
        this.ctr++
      }, 1000)
    },
    stopWaiting () {
      clearInterval(this.interval)
      this.$refs.waiting.innerHTML = ''
    },
    generate () {
      this.startWaiting()
      deepai.callStandardApi("text-generator", {
        text: this.inputText
      }).then(result => {
        this.stopWaiting()
        deepai.renderResultIntoElement(result, this.$refs.output);
      })
    }
  },
  created () {
    deepai.setApiKey('bc228f67-6b5d-4664-8b2c-4a7703846222')
  }
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

#output {
  margin: 2em;
  text-align: left;
}
</style>
