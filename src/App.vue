<template>
  <the-header></the-header>
  <section id="monster" class="container">
    <h2>Monster Health</h2>
    <div class="healthbar">
      <div class="healthbar__value" :style="monsterHealthBar"></div>
    </div>
  </section>
    <section id="player" class="container">
      <h2>Your Health</h2>
      <div class="healthbar">
         <div class="healthbar__value" :style="playerHealthBar"></div>
      </div>
  </section>
  <game-over
  :player-health="this.playerHealth"
  :monster-health="this.monsterHealth"
  :game-winner="this.gameWinner"
  @game-winner="hideButtons"
  @new-game="newGame"
  ></game-over>
  <game-buttons
  v-if="!isWinner"
  :round ="this.round"
  ></game-buttons>
  <battle-log
  :log-messages="this.logMessages"
  ></battle-log>
</template>

<script>
import TheHeader from './components/TheHeader.vue';
import GameButtons from './components/GameButtons.vue'
import BattleLog from './components/BattleLog.vue'
import GameOver from './components/GameOver.vue';

export default {
  name: 'App',
  components: {
    TheHeader,
    GameOver,
    GameButtons,
    BattleLog
  },
  data(){
        return{
            playerHealth: 100,
            monsterHealth: 100,
            round: 0,
            isWinner: false,
            gameWinner: null,
            logMessages: []
        }
    },
  provide() {
        return {
            attackButton: this.attackButton,
            specialAttackButton: this.specialAttackButton,
            healButton: this.healButton,
            surrenderButton: this.surrender,
        }
    },
    methods: {
      
        attackButton(who, what) {
            this.round ++
            const attackValue = this.getRandomValues(3, 8)
            this.monsterHealth -= attackValue;
            this.addLogMessages(who, what, attackValue)
            this.monstersAttack('Monster', 'attacked')
        },
        monstersAttack(who, what) {
          setTimeout(() => {
            const attackValue = this.getRandomValues(5, 8)
            this.playerHealth -= attackValue
            this.addLogMessages(who, what, attackValue)
          }, 100)
            
        },
        specialAttackButton(who, what){
          this.round ++
          const attackValue = this.getRandomValues(6, 13)
          this.monsterHealth -= attackValue;
          this.addLogMessages(who, what, attackValue)
          this.monstersAttack('Monster', 'attacked')
        },
        healButton(who, what) { 
          const healValue = this.getRandomValues(3, 14)
          if((this.playerHealth + healValue) > 100){
            let missingHP = 100 - this.playerHealth;
            this.playerHealth = 100;
            this.addLogMessages(who, what, missingHP)
            this.monstersAttack('Monster', 'attacked')
            return;
          }
          this.round ++
          this.playerHealth += healValue;
          this.addLogMessages(who, what, healValue)
          this.monstersAttack('Monster', 'attacked')
        },
        surrender(who, what) {
          this.gameWinner = 'monster'
          this.addLogMessages(who, what)
        },
        hideButtons(winner){
        this.isWinner = true
        this.gameWinner = winner
        },
        newGame() {
          this.playerHealth = 100;
          this.monsterHealth = 100;
          this.round = 0;
          this.isWinner = false;
          this.gameWinner = null;
          this.logMessages = [];
        },
        addLogMessages(who, what, value){
          this.logMessages.unshift({
            actionBy: who,
            actionType: what,
            actionValue: value
          })
        },
        getRandomValues(min, max){
            return Math.floor(Math.random() * (max - min + 1) + min);
        },
    },
    computed: {
      monsterHealthBar() {
        if(this.monsterHealth < 0) return {width: 0}
        return {width: this.monsterHealth + '%'}
      },
      playerHealthBar() {
        if(this.playerHealth < 0) return {width: 0};
        return {width: this.playerHealth + '%'}
      }
    }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
html {
  font-family: 'Jost', sans-serif;
}
body {
  margin: 0;
}
section {
  width: 90%;
  max-width: 40rem;
  margin: auto;
}
.container {
  text-align: center;
  padding: 0.5rem;
  margin: 1rem auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  border-radius: 12px;
}
.healthbar {
  width: 100%;
  height: 40px;
  border: 1px solid #575757;
  margin: 1rem 0;
  background: #fde5e5;
}
.healthbar__value {
  background-color: #00a876;
  width: 100%;
  height: 100%;
}
#monster h2,
#player h2 {
  margin: 0.25rem;
}
</style>
