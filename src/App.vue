<template>
  <div id="app">
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">YOU</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            style="background-color: green; margin: 0; color: white;"
            :style="{width: playerHealth + '%'}"
          >{{ playerHealth }}</div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">MONSTER</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            style="background-color: green; margin: 0; color: white;"
            :style="{width: monsterHealth + '%'}"
          >{{ monsterHealth }}</div>
        </div>
      </div>
    </section>
    <section class="row controls" v-if="!gameIsRunning">
      <div class="small-12 columns">
        <button id="start-game" @click="startGame">START NEW GAME</button>
      </div>
    </section>
    <section class="row controls" v-else>
      <div class="small-12 columns">
        <button id="attack" @click="attack">ATTACK</button>
        <button id="special-attack" @click="specialAttack">SPECIAL ATTACK</button>
        <button id="heal" @click="heal">HEAL</button>
        <button id="give-up" @click="giveUp">GIVE UP</button>
      </div>
    </section>
    <section class="row log">
      <div class="small-12 columns">
        <ul>
          <li></li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      gameIsRunning: false
    };
  },
  methods: {
    startGame: function() {
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.gameIsRunning = true;
    },
    calcRandom: function(min, max) {
      return Math.max(Math.floor(Math.random() * max) + 1, min);
    },
    playerAttack: function(min, max) {
      this.monsterHealth -= this.calcRandom(min, max);
      this.checkMonster();
    },
    monsterAttack: function() {
      this.playerHealth -= this.calcRandom(5, 12);
      this.checkPlayer();
    },
    checkMonster: function() {
      if (this.monsterHealth <= 0) {
        if (confirm("You won! New game?")) {
          this.startGame();
        } else {
          this.gameIsRunning = false;
        }
      }
    },
    checkPlayer: function() {
      if (this.playerHealth <= 0) {
        if (confirm("You died. New game?")) {
          this.startGame();
        } else {
          this.gameIsRunning = false;
        }
      }
    },
    attack: function() {
      this.playerAttack(3, 10);
      this.monsterAttack();
    },
    specialAttack: function() {
      this.playerAttack(10, 20);
      this.monsterAttack();
    },
    heal: function() {
      this.playerHealth += this.calcRandom(10, 20);
      if (this.playerHealth > 100) {
        this.playerHealth = 100;
      }
      this.monsterAttack();
    },
    giveUp: function() {
      this.gameIsRunning = false;
    }
  }
};
</script>
  
<style>
@import "./foundation.min.css";
@import "./app.css";
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
