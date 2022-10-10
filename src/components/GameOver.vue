<template>
<section class="container" v-if="winner">
        <h2>Game Over!</h2>
        <h3 v-if="winner === 'player'">You won!</h3>
        <h3 v-else-if="winner === 'monster'">You lost!</h3>
        <h3 v-else-if="winner === 'draw'">It's draw!</h3>
        <button @click="startNewGame">Start new game</button>
      </section>
</template>

<script>


export default {
    props: ['playerHealth', 'monsterHealth', 'gameWinner'],
    emits: ['game-winner'],
    data() {
        return{
            winner: null,
        }
    },
    methods: {
        startNewGame() {
            this.$emit('newGame')
            this.winner = null
        }
    },
    watch: {
        playerHealth(value){
            if(value <= 0 && this.monsterHealth <= 0){
                this.winner = 'draw';
                this.$emit('game-winner', this.winner)
            } 
            else if(value > 0 && this.monsterHealth <= 0){
                this.winner = 'player';
                this.$emit('game-winner', this.winner)
            }
            else if(value <= 0 && this.monsterHealth > 0){ 
            this.winner = 'monster';
            this.$emit('game-winner', this.winner)
            }
        },
        gameWinner(value){
            if(value !== 'monster') return;
            this.winner = 'monster';
            this.$emit('game-winner', this.winner)
        }
    }
}
</script>

<style>
button {
  font: inherit;
  border: 1px solid #88005b;
  background-color: #88005b;
  color: white;
  padding: 1rem 2rem;
  border-radius: 12px;
  margin: 1rem;
  width: 12rem;
  cursor: pointer;
  box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
}

button:focus {
  outline: none;
}

button:hover,
button:active {
  background-color: #af0a78;
  border-color: #af0a78;
  box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.26);
}
</style>