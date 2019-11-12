<template>
  <div class="container">
      <div class="row">
          <div class="col-md-9" style="border-right: medium solid #ccc">
            <div class="row" @mousedown.once="startClock">
              <h2>Pickup Cards</h2>
              <draggable class="list-group" :list="cards" group="cards" @end="log" :sort="false" :move="checkMove" direction="vertical">
                  <div class="card " v-for="(card, index) in cards" :key="card.id" :id="card.id"
                      @click="checkCard(card.id)">
                      <div class="flip-card-inner">
                        <div class="flip-card-front">
                          <div class="card-body">
                              <div class="card-title">?</div>
                          </div>
                        </div>
                        <div class="flip-card-back">
                          <div class="card-body">
                              <img :src="require('../assets/' + card.img)">
                          </div>
                        </div>
                      </div>
                  </div>
                  <!-- End loop -->
              </draggable>
            </div>
              <!--
            Begin Drop Zone
          -->
          <div class="row" style="margin-top: 10vh; padding-top:10vh; border-top: medium solid #ccc">
            <h2>Zoovu Logo</h2>
              <!-- Z -->
              <div class="landing">
                  <draggable class="z card" :list="z" group="cards" @change="log" @add="checkComplete" direction="vertical">
                      <div class="" v-for="(card, index) in z" :key="card.id">
                          <img :src="require('../assets/' + card.img)">
                      </div>
                  </draggable>
              </div>
              <!-- O -->
              <div class="landing">
                  <draggable class="o card" :list="o" group="cards" @change="log" @add="checkComplete" direction="vertical">
                      <div class="" v-for="(card, index) in o" :key="card.id">
                          <img :src="require('../assets/' + card.img)">
                      </div>
                  </draggable>
              </div>
              <!-- O -->
              <div class="landing">
                  <draggable class="O card" :list="O" group="cards" @change="log" @add="checkComplete" direction="vertical">
                      <div class="" v-for="(card, index) in O" :key="card.id">
                          <img :src="require('../assets/' + card.img)">
                      </div>
                  </draggable>
              </div>
              <!-- V -->
              <div class="landing">
                  <draggable class="v card" :list="v" group="cards" @change="log" @add="checkComplete" direction="vertical">
                      <div class="" v-for="(card, index) in v" :key="card.id">
                          <img :src="require('../assets/' + card.img)">
                      </div>
                  </draggable>
              </div>
              <!-- U -->
              <div class="landing">
                  <draggable class="u card" :list="u" group="cards" @change="log" @add="checkComplete" direction="vertical">
                      <div class="" v-for="(card, index) in u" :key="card.id">
                          <img :src="require('../assets/' + card.img)">
                      </div>
                  </draggable>
              </div>
            </div>
          </div>
          <div class="col-md-3">
          <div id="timer">Your Score: <b>{{ time }}</b> </div>
              <div id="findCard" class="">
                <h3>Find This Card:</h3>
                  <div id="look" :class="card.id + ' card'" v-for="(card, i) in cards" :key="card.id" v-if="i === look">
                      <div class="card-body">
                          <img :src="require('../assets/' + card.img)">
                      </div>
                  </div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import Cards from '@/assets/cards.json'
import draggable from 'vuedraggable'
import _ from 'lodash'
const images = require.context('@/assets', false, /\.png$|\.jpg$/)
export default{
  components: {
    draggable
  },
  data () {
    return {
      cards: _.shuffle(Cards),
      look: 0,
      z: [],
      o: [],
      O: [],
      v: [],
      u: [],
      timer: 0,
      time: 0
    }
  },
  methods: {
    loadImg (imgPath) {
      this.randomNumber()
      return images('./' + imgPath)
    },
    checkCard (id) {
      document.getElementById(id).classList.add('flip')
      if (document.getElementById('look').classList.contains(id)) {
        document.getElementById(id).classList.add('flip')
        // this.randomNumber()
      } else {
        setTimeout(() => {
          document.getElementById(id).classList.toggle('flip')
        }, 1500)
        this.time += 10
        this.randomNumber()
      }
    },
    startClock () {
      if (this.time === 0) {
        this.timer = setInterval(() => {
          this.time += 1
        }, 1000)
      }
    },
    log: function (evt) {
      // console.log(evt)
      if (evt.from === evt.to) {
        // console.log('Back Home')
        // this.time += 10
      } else {
        // console.log('New Home')
        // console.log(this.cards)
        // console.log(this.randomA)
        this.randomNumber()
      }
    },
    checkComplete: function (evt) {
      this.gameComplete()
    },
    checkMove: function (evt) {
      let classList = evt.relatedContext.component.rootContainer.classList
      let letter = evt.draggedContext.element.letter
      if (!classList.contains(letter)) {
        return false
      }
    },
    gameComplete () {
      if (this.cards === undefined || this.cards.length === 0) {
        // notice('Game Complete')
        clearInterval(this.timer)
        this.popUp()
        setTimeout(function () {
          location.reload()
        }, 10000)
      }
    },
    randomNumber () {
      console.log('Array Length: ' + this.cards.length)
      this.look = Math.floor(Math.random() * this.cards.length)
      console.log('Random Num: ' + this.look)
    },
    popUp () {
      let pop = document.createElement('div')
      pop.innerHTML += `
      <div id="popup">
        <h2 style="text-align: center;">You Beat The Game!</h2>
        <p>The game will be reset in 10 seconds</p>
        <p>Your score was: <b>${this.time}</b>
      </div>
      `
      document.body.append(pop)
    }
  },
  mounted () {
  }
}
</script>
<style>
body{
  padding-top:50px;
}
  .landing .card{
    background: rgba(0,0,0,0.1);
    transition: all .5s ease-in-out;
    margin-bottom: 5px;
    width:150px;
    min-height: 200px;
    padding:25px;
  }
  #findCard img{
    max-width:100%;
  }
  .card, .landing{
    max-width: 150px;
    flex-direction: row;
    margin:5px;
    width: 150px;
    min-height: 250px;
    cursor: pointer;
  }
  .card img, .landing img{
    max-width:100%;
    height: auto;
  }
  .list-group{
    flex-direction: row;
  }
  /* This container is needed to position the front and back side */
.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

/* Do an horizontal flip when you move the mouse over the flip box container */
.flip .flip-card-inner {
  transform: rotateY(180deg);
}

/* Position the front and back side */
.flip-card-front, .flip-card-back {
  position: absolute;
  width: 100%;
  height: 100%;cd practice
  backface-visibility: hidden;
}

/* Style the front side (fallback if image is missing) */
.flip-card-front {
  background-color: #bbb;
  color: #fff;
  font-size: 10rem;
  line-height: 1;
}

/* Style the back side */
.flip-card-back {
  background-color: dodgerblue;
  color: white;
  transform: rotateY(180deg);
}
#popup{
  text-align: center;
  position: absolute;
  padding: 100px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #2d2d2d;
  color: #fff;
  border-radius: 5px;
  box-shadow: 0px 10px 25px 0px rgba(0,0,0,0.75);
}
#timer{
  font-size: 2rem;
  text-align: center;
  padding-bottom: 25px;
  border-bottom: medium solid #ccc;
  margin-bottom: 25px
}
#findCard{
  margin: 0 auto;
  text-align: center;
}
#look{
  margin: 0 auto;
}
h2{
  width: 100%
}
</style>
