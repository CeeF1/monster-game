<template>
  <section id="controls">
        <button @click="attackButton('You', 'attacked')">ATTACK</button>
        <button @click="specialAttackButton('You', 'special-attacked')" :disabled="disabledSpecialAttack">SPECIAL ATTACK</button>
        <button @click="healButton('You','healed')">HEAL</button>
        <button @click="surrenderButton('You', 'gave up')">SURRENDER</button>
      </section>
</template>

<script>
export default {
    inject: ['attackButton', 'specialAttackButton', 'healButton', 'surrenderButton'],
    props: ['round'],
    computed: {
        disabledSpecialAttack() {
            return this.round % 3 != 0;
        },
    },
}
</script>

<style scoped>
#controls {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}

button {
  font: inherit;
  border: 1px solid #88005b;
  background-color: #88005b;
  color: white;
  padding: 1rem 2rem;
  border-radius: 12px;
  margin: 1rem;
  width: 13rem;
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

button:disabled {
  background-color: #ccc;
  border-color: #ccc;
  box-shadow: none;
  color: #3f3f3f;
  cursor: not-allowed;
}

</style>