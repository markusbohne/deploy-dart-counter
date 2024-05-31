<script setup>
  import {computed, ref} from "vue";
  import { checkouts } from "./constants/checkouts.js"



  const playerOnePointsLeft = ref(501)
  const playerOnePointsLeftSAFE = ref()
  const playerTwoPointsLeft = ref(501)
  const playerTwoPointsLeftSAFE = ref()

  const multiplier = ref("")

  const turnCount = ref(3)
  const turn = computed(() => {
    if(turnCount.value === -3) {
      playerTwoPointsLeftSAFE.value = playerTwoPointsLeft.value
      turnCount.value = 3
      return 'player1'
    } else {
      if(turnCount.value === 0){
        playerOnePointsLeftSAFE.value = playerOnePointsLeft.value
      }
      return turnCount.value > 0 ? 'player1' : 'player2'
    }
  })

  const checkoutWay = computed(() => {
    if(turn.value === "player1" && playerOnePointsLeft.value <= 170) {
      return checkouts[playerOnePointsLeft.value.toString()] || "-"
    } else if (turn.value === "player2" && playerTwoPointsLeft.value <= 170) {
      return checkouts[playerTwoPointsLeft.value.toString()] || "-"
    }
    return "-"
  })

  function setMultiplier(value) {
    if(multiplier.value === value){
      multiplier.value = ''
      return
    }
    multiplier.value = value
  }

  function decrease(score) {
    let multi = 1
    if(multiplier.value === 'D' && score <= 20) {
      multi = 2
    } else if(multiplier.value === 'T' && score <= 20){
      multi = 3
    }
    if(turn.value === 'player1') {
      playerOnePointsLeft.value = playerOnePointsLeft.value - (score * multi)
      if(playerOnePointsLeft.value < 0 || playerOnePointsLeft.value === 1) {
        playerOnePointsLeft.value = playerOnePointsLeftSAFE.value
        turnCount.value = 1
      }
      if(playerOnePointsLeft.value === 0 && multi === 2) {
        window.alert("player1 won")
      }
    } else {
      playerTwoPointsLeft.value = playerTwoPointsLeft.value - (score * multi)
      if(playerTwoPointsLeft.value < 0 || playerTwoPointsLeft.value === 1) {
        playerTwoPointsLeft.value = playerTwoPointsLeftSAFE.value
        turnCount.value = 4
      }
      if(playerTwoPointsLeft.value === 0 && multi === 2) {
        window.alert("player2 won")
      }
    }
    turnCount.value--
    multiplier.value = ''
  }

  function resetGame() {
    playerOnePointsLeft.value = 501
    playerTwoPointsLeft.value = 501
    if(turnCount.value === 3) {
      turnCount.value = 0
    } else {
      turnCount.value = 3
    }
}
</script>

<template>
  <div class="site">
    <h1 class="playerOnePointsLeft" :play-one-status="turn">{{playerOnePointsLeft}}</h1>
    <h1 class="playerTwoPointsLeft" :play-two-status="turn">{{playerTwoPointsLeft}}</h1>
    <h1 class="checkoutWay">{{checkoutWay}}</h1>
    <div class="button-container">
      <button v-for="n in 20" class="button" @click="decrease(n)">{{multiplier}}{{n}}</button>
      <button v-for="n in 2" class="button" @click="decrease(n*25)">{{n*25}}</button>
      <button class="button" @click="decrease(0)">0</button>
    </div>
    <div class="multiply-container">
      <button class="text-button" @click="setMultiplier('D')">Double</button>
      <button class="text-button" @click="setMultiplier('T')">Triple</button>
    </div>
    <button class="resetButton" @click="resetGame">Reset Game</button>
  </div>
</template>

<style scoped>
  .site {
    display: grid;
    grid-template-columns: 50%;
    justify-items: center;
  }

  .multiply-container {
    margin-top: 2rem;
    margin-bottom: 2rem;
    grid-column: 1 / 3;
    grid-row: 3;
  }

  .playerOnePointsLeft {
    grid-column: 1;
    grid-row: 1;
    height: 6rem;
    width: 12rem;
    border: black solid 2px;
    text-align: center;
    align-content: center;
    font-size: 80px;
  }

  .playerTwoPointsLeft {
    grid-column: 2;
    grid-row: 1;
    height: 6rem;
    width: 12rem;
    border: black solid 2px;
    text-align: center;
    align-content: center;
    font-size: 80px;
  }

  .checkoutWay {
    grid-row: 2;
    grid-column: 1 / 3;
  }

  .button-container {
    grid-column: 1 / 3;
    grid-row: 4;
    margin-bottom: 50px;
    border: 1px solid black;
  }

  .button {
    border: none;
    margin: 5px;
    width: fit-content;
    min-width: 11rem;
    height: fit-content;
    min-height: 5rem;
    font-size: 50px;
    cursor: pointer;
  }

  .text-button {
    border: none;
    margin: 5px;
    width: fit-content;
    min-width: 30rem;
    height: fit-content;
    min-height: 8rem;
    font-size: 60px;
  }

  .resetButton {
    grid-row: 5;
    justify-self: start;
    margin: 5px;
    width: 10rem;
    height: 3rem;
    font-size: x-large;
  }

  [play-one-status="player1"] {
    color: green;
    border-color: green;
  }
  [play-two-status="player2"] {
    color: green;
    border-color: green;
  }
</style>