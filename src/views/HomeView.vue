<template>
  <div class="home">
    <div class="buttons">
      <button @click="halfC" v-if="!court"> Half Court?</button> 
      <button @click="fullC" v-if="court"> Full Court?</button>
    </div>
       
    <div class="content">
      <div class="ball-wrapper">
        <div class="ball"></div>
        <div class="shadow"></div>
      </div>

      <HalfTimer v-if="court"/>
      <FullTimer v-if="!court"/>

    </div>
  </div>
</template>

<script>
import FullTimer from '../components/FullTimer.vue'
import HalfTimer from '../components/HalfTimer.vue'

export default {
  name: 'HomeView',
  components: { FullTimer, HalfTimer },
  data() {
    return {
      court: false
    }
  },
    methods: {
      halfC() {
          this.court = !this.court
      },
      fullC() {
            this.court = !this.court
      }
    }
}
</script>

<style scoped>
.home {
  padding: 10px;
}
.content {
  margin: 140px auto;
}
.buttons {
  position: relative;
  top: 70px;
}
</style>

