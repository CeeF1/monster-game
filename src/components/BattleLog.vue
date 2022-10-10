<template>
  <section id="log" class="container">
        <h2>Battle Log</h2>
        <ul v-for="logMessage in logMessages" :key="logMessage">
          <span :class="{'log--player': logMessage.actionBy === 'You', 'log--monster': logMessage.actionBy === 'Monster'}">
            {{logMessage.actionBy + ' '}}
          </span>
          <span v-if="logMessage.actionType !== 'gave up'">
            {{logMessage.actionType + ' for '}}
            <span :class="{'log--damage': logMessage.actionType !== 'heal', 'log--heal': logMessage.actionType === 'healed'}">
              {{logMessage.actionValue}}
            </span>
          </span>
          <span v-else-if="logMessage.actionType === 'gave up'">
            {{logMessage.actionType}}
          </span>
        </ul>
    </section>
</template>

<script>
export default {
  props: ['logMessages'],
  data() {
    return{
      
    }
  }
}
</script>

<style>
#log ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

#log li {
  margin: 0.5rem 0;
}

.log--player {
  color: #003cff;
}

.log--monster {
  color: #ff7402;
}

.log--damage {
  color: red;
}

.log--heal {
  color: rgb(0, 160, 0);
}
</style>