<template>
  <h1>Enter a prompt</h1>
  <input type="text" v-model="inputText">
  <button @click="generate">Generate</button>
  <h1>GPT-2 continuation</h1>
  <h2 ref="waiting"></h2>
  <div id="output" ref="output">{{ outputText }}</div>
</template>

<script>
const host = "localhost"
const port = 8000
const httpProtocol = "http"
const apiURL = `${httpProtocol}://${host}:${port}`

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
      this.$refs.output.innerHTML = ''
      this.startWaiting()

      const requestOptions = {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ prompt: this.inputText })
      }
      return fetch(`${apiURL}/generate`, requestOptions)
      .then(response => {
          return response.json()
      })
      .then(result => {
        this.stopWaiting()
        this.$refs.output.innerHTML = result.result[0]
      })
    }
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
