<template>
  <div class="content">
    <div class="board">
      <p>Simon Game</p>
      <div class="button-red"
           :class="{ red: activityColor.red }"
           @click="activateColor('red')">
      </div>
      <div class="button-blue"
           :class="{ blue: activityColor.blue  }"
           @click="activateColor('blue')"></div>
      <div class="button-yellow"
           :class="{ yellow: activityColor.yellow }"
           @click="activateColor('yellow')"></div>
      <div class="button-green"
           :class="{ green: activityColor.green }"
           @click="activateColor('green')"></div>
    </div>
    <div class="level">
      <h3 class="title-level">Cложность:</h3>
      <div>
        <input type="radio" id="easy" value="easy" v-model="currentLevel">
        <label for="easy">Лёгкий</label>
      </div>
      <div>
        <input type="radio" id="medium" value="medium" v-model="currentLevel">
        <label for="medium">Средний</label>
      </div>
      <div>
        <input type="radio" id="hard" value="hard" v-model="currentLevel">
        <label for="hard">Cложный</label>
      </div>
      <button class="start"
              @click="start()"
              v-if="!started">Старт</button>
      <h3 class="lose" v-if="finished">Вы проиграли! Попробуйте ещё раз.</h3>
    </div>
  </div>
</template>

<script>
export default {
  name: "SimonGame",
  data() {
    return {
      gameArray: [], // загорающиеся квадраты
      playerArray: [], // квадраты, нажатые пользователем
      started: false,
      finished: false,
      number: 4, // кол-во квадратов на поле
      colors: {
        1: 'red',
        2: 'blue',
        3: 'yellow',
        4: 'green',
      },
      currentColor: '',
      activityColor: {
        red: false,
        blue: false,
        yellow: false,
        green: false,
      },
      level: {
        easy: 1500,
        medium: 1000,
        hard: 400,
      },
      currentLevel: 'easy',
      clicked: false, // был ли квадрат нажат игроком
    }
  },
  methods: {
    start() {
      this.started = true;
      this.finished = false;
      this.simonGame(this.getCurrentLevel());
    },
    simonGame(level) {
      this.gameArray.push(this.random(1, this.number)); // выбор одного рандомного квадрата
      this.gameArray.forEach((square, i) => {
        setTimeout(() => {
          let color = this.colors[square]; // загорание квадрата
          this.activityColor[color] = true;

          setTimeout(() => {
            this.activityColor[color] = false // затухание квадрата
          }, 500)
        }, (i + 1) * level)
      })
    },
    random(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    },
    activateColor(color) { // загорание квадрата, нажатым игроком
      this.activityColor[color] = true;
      this.clicked = true;
      this.currentColor = color
      setTimeout(() => {
        this.activityColor[color] = false
        this.clicked = false
      }, 200)
    },
    getColor(object, value) {
      return Object.keys(object).find(key => object[key] === value);
    },
    checkPlayerArray() { // проверка правильности последовательности квадратов, нажатых игроком
      console.log('check')
      this.playerArray.forEach((square, i) => {
        if (square !== this.gameArray[i]) {
          this.finished = true;
          return;
        }
      })

      if (this.playerArray.length === this.gameArray.length && !this.finished) {
        this.playerArray.length = 0
        this.start()
      }
    },
    getCurrentLevel() {
      return this.level[this.currentLevel]
    },
  },
  watch: {
    clicked() {
      if (this.clicked) {
        let value = this.getColor(this.colors, this.currentColor)
        console.log(value)
        this.playerArray.push(Number(value))
        this.checkPlayerArray()
      }
    },
    finished() {
      if (this.finished) {
        this.started = false
        this.gameArray.length = 0
        this.playerArray.length = 0
      }
    },
    currentLevel() {
      this.started = false
      this.gameArray.length = 0
      this.playerArray.length = 0
    },
  },
}

</script>

<style lang="sass">
.content
  display: flex
  justify-content: center

p
  text-align: center
  font-size: 2rem
  font-weight: bold

.title-level
  margin-top: 200px
  width: 350px

label
  padding-left: 3px

.start
  border: 1px solid #4F4F4F
  padding: 10px 30px
  border-radius: 3px
  font-size: 0.9em
  cursor: pointer
  margin: 15px 0 30px
  font-weight: bold
  background: #C7C7C7

.start:hover
  background: #E6E6E6

.board
  padding: 5px
  margin: 50px 100px 50px 0
  width: 420px

.button-red
  background: #FFB0AD
  height: 190px
  width: 190px
  margin: 10px
  float: left
  cursor: pointer
  z-index: 2
  transition: background-color 0.3s ease

.red
  background: red

.button-blue
  height: 190px
  width: 190px
  margin: 10px
  float: left
  cursor: pointer
  z-index: 2
  transition: background-color 0.3s ease
  background: #A7C8FB

.blue
  background: #0A1CFF

.button-yellow
  height: 190px
  width: 190px
  margin: 10px
  float: left
  cursor: pointer
  z-index: 2
  transition: background-color 0.3s ease
  background: #FFFBCC

.yellow
  background: #FFF205

.button-green
  height: 190px
  width: 190px
  margin: 10px
  float: left
  cursor: pointer
  z-index: 2
  transition: background-color 0.3s ease
  background: #B8F5AD

.green
  background: #06FA00

@media (max-width: 900px)
  .content
    flex-direction: column
    justify-content: center
    align-items: center

  .board
    margin: 0

  .level
    padding-right: 50px
  .title-level
    margin-top: 5px

@media (max-width: 500px)
  p
    font-size: 1.7rem
  .board
    width: 330px

  .button-red, .button-blue, .button-yellow, .button-green
    width: 140px
    height: 140px

  .level
    padding-left: 90px

</style>