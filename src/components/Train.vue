<template>
  <div>
    <div>
      <pre>
        <code>
{{ time }}
        </code>
      </pre>
    </div>
    <pre>
      <code>
        {{ animateTrain }}
        <div style="margin-top: -60px">
        {{ rails }}
        </div>
      </code>
    </pre>
  </div>
</template>

<script>
import { render } from 'cfonts'
import moment from 'moment'
import { train, sprites } from './sprites'

export default {
  data() {
    return {
      time: '',
      spriteCounter: 0,
      offset: 4,
      counter: 0,
      limit: 195,
      rails: new Array(125).fill('_ ').join(''),
      train,
      sprites,
      updateTrainSpriteInterval: null, 
      updateTimeInterval: null, 
      opts: {
        align: 'left',
        font: 'simpleblock'
      }
    }
  },
  computed: {
    animateTrain () {
      let newTrain = this.train.split('\n')
      for (let index = 0; index < newTrain.length; index++) {
        newTrain[index] = this.fillWithSpaces(newTrain[index], this.counter);
      }
      newTrain = this.joinLines(newTrain)
      return newTrain
    }
  },
  created () {
    this.updateTrainSpriteInterval = setInterval(() => {
      this.updateTrainSprite()
    }, 400)
    this.updateTimeInterval = setInterval(() => {
      this.updateTrain()
      this.updateTime()
    }, 1000)
  },
  destroyed () {
    clearInterval(this.updateTrainSpriteInterval)
    clearInterval(this.updateTimeInterval)
  },
  methods: {
    async updateTime () {
      const now = new Date()
      const year = now.getFullYear()
      const christmas = moment(new Date(`${year}-12-25 00:00:00`))
      let message = this.render(`${moment(christmas).diff(now, 'seconds')}`, this.opts)
      const diff = moment(now).diff(christmas)
      if (diff >= 0 && diff <= 25200000) {
        message = this.render('Feliz', this.opts)
        message = message + '\n' + this.render('Navidad!', this.opts)
      }
      this.time = message
    },
    updateTrain () {
      this.counter += this.offset
      if (this.counter > this.limit) {
        this.counter = 0
      }
    },
    updateTrainSprite () {
      this.spriteCounter++
      if (this.spriteCounter == this.sprites.length){
        this.spriteCounter = 0
      }
      this.train = this.sprites[this.spriteCounter]
    },   
    fillWithSpaces (line, n) {
      for (let index = 0; index < n; index++) {
        line = ' ' + line  
      }
      return line
    },
    joinLines (lines) {
      let result = ''
      for (let index = 0; index < lines.length; index++) {
        result = result + '\n' + lines[index]
      }
      return result
    },
    render (text, opts) {
      return render(text, opts).string
    }
  }
}
</script>
<style lang="scss">
code {
  background-color: black;
  color: rgb(45, 223, 45);
}
</style>