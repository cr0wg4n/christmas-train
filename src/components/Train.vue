<template>
  <div>
    <div style="margin-left: 35%">
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
const CFonts = require('cfonts')
import moment from 'moment'

export default {
  data() {
    return {
      time: '',
      spriteCounter: 0,
      offset: 4,
      counter: 0,
      limit: 195,
      rails: '_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ ',
      train: `
o        o      o    o   o   o   oo  oo  oo  oo}
    o     o  oo  ooo  oo  ooo ooo  ooo ooo ooo oooooo}
                  oo  oo   oo  oo  ooo ooo ooo oooo ooooo}
                        o    o   o   oo  oo  oo  oooo oooooo}
                                        o   o   o   ooo  oo ooooo}
                                                            o ooo}
                                _________            \\|/         ++
                    ___________ |_______|______/-\\___|-|_^_/-\\___||_n
            n_____/    OOO    \\_ |[][] |----------------------|----|\\
            |          (_)      ||     |______________________|_|| ||P
            |          OOO      | -----/====+______|| _---====--||- /
            q|===================|_|=_/o====+|=====[__]o====+|==[__]-p\\
              (_)(_)      (_)(_)      \\__/ \\__/ \\__/  \\__/ \\__/ (_)____\\
      `,
      sprites: [`
o        o      o    o   o   o   oo  oo  oo  oo}
    o     o  oo  ooo  oo  ooo ooo  ooo ooo ooo oooooo}
                  oo  oo   oo  oo  ooo ooo ooo oooo ooooo}
                        o    o   o   oo  oo  oo  oooo oooooo}
                                        o   o   o   ooo  oo ooooo}
                                                            o ooo}
                                _________            \\|/         ++
                    ___________ |_______|______/-\\___|-|_^_/-\\___||_n
            n_____/    OOO    \\_ |[][] |----------------------|----|\\
            |          (_)      ||     |______________________|_|| ||P
            |          OOO      | -----/====+______|| _---====--||- /
            q|===================|_|=_/o====+|=====[__]o====+|==[__]-p\\
              (_)(_)      (_)(_)      \\__/ \\__/ \\__/  \\__/ \\__/ (_)____\\
      `,`
        o      o    o          oo  oo  oo  oo}
    o     o  oo     oo                oo ooo oooo  }
                  oo  oo   oo  oo  o   oo ooo oooo ooooo}
                        o    o   o   o   oo            ooooo}
                                        o   o   o     o  oo ooo}
                                                        o      oo}
                                _________            \\|/         ++
                    ___________ |_______|______/-\\___|-|_^_/-\\___||_n
            n_____/    OOO    \\_ |[][] |----------------------|----|\\
            |          (_)      ||     |______________________|_|| ||P
            |          OOO      | -----/====+______|| _---====--||- /
            q|===================|_|=_/o====+|=====[__]o====+|==[__]-p\\
              (_)(_)      (_)(_)      \\__/ \\__/ \\__/  \\__/ \\__/ (_)____\\
      `, `
o        o      o    o  ooooo   oo  oo   oo}
          o  oo     oo       oooo      oo ooo o o }
                  oo  oo   oo  oo  o   oo oo     ooo  o}
                        o    o   o   oo  oo            ooooo}
                                    o        o   o   ooo  oo oo}
                                                            o o  }
                                _________            \\|/         ++
                    ___________ |_______|______/-\\___|-|_^_/-\\___||_n
            n_____/    OOO    \\_ |[][] |----------------------|----|\\
            |          (_)      ||     |______________________|_|| ||P
            |          OOO      | -----/====+______|| _---====--||- /
            q|===================|_|=_/o====+|=====[__]o====+|==[__]-p\\
              (_)(_)      (_)(_)      \\__/ \\__/ \\__/  \\__/ \\__/ (_)____\\
      `,`
oo  o      oo       o    o  ooooo   oo  oo   oo}
          o  oo     oo       ooooo o o o o     o o }
              o o     oooo   oo  oo  o   oo oo     oo o}
                        o    o   o   oo  oo        o      o}
                                    o        o   o   ooo  oo oo}
                                                            o o  }
                                _________            \\|/         ++
                    ___________ |_______|______/-\\___|-|_^_/-\\___||_n
            n_____/    OOO    \\_ |[][] |----------------------|----|\\
            |          (_)      ||     |______________________|_|| ||P
            |          OOO      | -----/====+______|| _---====--||- /
            q|===================|_|=_/o====+|=====[__]o====+|==[__]-p\\
              (_)(_)      (_)(_)      \\__/ \\__/ \\__/  \\__/ \\__/ (_)____\\
      `
      ]
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
    setInterval(() => {
      this.updateTrain()
    }, 1000)
    setInterval(() => {
      this.updateTrainSprite()
    }, 500)
    setInterval(() => {
      this.updateTime()
    }, 1000)
  },
  methods: {
    async updateTime () {

      const now = new Date()
      const year = now.getFullYear()
      const christmas = moment(new Date(`${year}-12-25 00:00:00`))
      const opts = {
        align: 'left',
        font: 'simpleblock',
      }
      let message = this.render(`${moment(christmas).diff(now, 'seconds')}`, opts)
      const diff = moment(now).diff(christmas)
      if (diff >= 0 && diff <= 25200) {
        message = this.render('Feliz', opts)
        message = message + '\n' + this.render('Navidad!', opts)
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
      return CFonts.render(text, opts).string
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