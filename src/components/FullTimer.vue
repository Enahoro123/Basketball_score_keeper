<template>
  <div class="timer">
    <div class="left">
        <img src="../assets/wilder.png" alt="">
    </div>
    <div class="middle">
        <div class="cont">

            <h1 class="lets"> Let's Ball Full-Court</h1>
                <h1>{{ timer.duration.minutes }} : {{ timer.duration.seconds }}</h1>

                <div v-for="option in options" :key="option.key" >
                    <p>{{ option.title }}</p>
                    
                    <button @click="subTo(option.key)">-</button>

                    <button @click="addTo(option.key)"> + </button><br />

                    <button  @click.prevent="startTimer" v-if="!isRunning">Start Timer</button>

                    <button @click="pauseTimer" v-if="isRunning">Pause Timer</button> 
                </div>
                
                <p class="score"><span :class="{ first }">{{ teamOne }}</span> : <span :class="{ sec }">{{ teamTwo }}</span> </p>

                <form>
                    <input type="number" v-model="scoreSet" placeholder="Set score">

                    <button @click.prevent="reset">Reset Game</button>
                </form>          

                <div v-if="!gameOn">
                    <button @click="toggleStart">Start New Game</button>
                </div>

                <div v-else>
                    <div class="player1">
                        
                        <h1>Team One</h1>
                        <div class="buttons">
                                <button @click="addfirst">+2</button> 
                                <button @click="addfirst3">+3</button>
                                <button @click="removeFirst">-2</button> 
                                <button @click="removeFirst3">-3</button>
                        </div>
                        
                    </div>
                        
                    <div class="player2">
                        
                        <h1>Team Two</h1>
                        <div class="buttons">
                            <button @click="addSecond2">+2</button>
                            <button @click="addSecond">+3</button>
                            <button @click="removeSecond2">-2</button>
                            <button @click="removeSecond">-3</button>
                        </div> 

                    </div>
                </div>
        </div>  
    </div>            

    <div class="right">
        <img class="image2" src="../assets/Friendly.png" alt="">
    </div>
      
  </div>    
  <div v-if="showModal">
        <Modal @close="toggleModal" theme="sale">
            <p class="score"><span :class="{ first }">{{ teamOne }}</span> : <span :class="{ sec }">{{ teamTwo }}</span> </p>
            <h1>{{ message }}</h1>
            <p>{{   mini    }}</p>
        </Modal>
  </div>
</template>

<script>
import Modal from '../components/Modal.vue'

export default {
    components: { Modal },
    data() {
        return {
            message: '', mini: '', teamOne: 0, teamTwo: 0, scoreSet: '', 
            first: false, sec: false, gameOn: false, showModal: false, 
            isRunning: false,
            timer: {
                duration: {
                    minutes: 10,
                    seconds: 0
                }
            },
            timerInt: null,
            options: [
                {
                    key: 0,
                    title: 'Set Timer',
                    lengthInMinutes: 10
                }
            ]

        }
    },

    methods: {
        MinToSec() {
            this.timer.duration.minutes--
            this.timer.duration.seconds = 59
        },

        timerUtils() {
            this.timerInt = setInterval(() => {
                if (
                    this.timer.duration.minutes === 0 &&
                    this.timer.duration.seconds === 0
                ) {
                    this.resetTimer()
                    this.showModal = true
                }
                if (this.timer.duration.seconds === 0 ) {
                    this.MinToSec()
                } else {
                    this.timer.duration.seconds--
                  }
                },
                1000)
        },

        startTimer() {
            this.isRunning = true;
            this.timerUtils()
        },

        pauseTimer() {
            this.isRunning = false;
            clearInterval(this.timerInt)
            console.log(this.timer.duration.minutes + ':' + this.timer.duration.seconds)
        },
        
        resetTimer() {
            this.isRunning = false;
            clearInterval(this.timerInt)
            this.timer.duration.seconds = 0
            this.timer.duration.minutes = this.options[0].lengthInMinutes
        },

        addTo(target) {
            if (this.options[target].lengthInMinutes <= 99) {
                this.options[target].lengthInMinutes++
            }
            if (this.options[target].key == 0) {
            this.timer.duration.minutes++
            }
        },

        subTo(target) {
        if (this.options[target].lengthInMinutes > 0) {
            this.options[target].lengthInMinutes--
        }
        if (this.options[target].key == 0) {
            this.timer.duration.minutes--
            }
        },

        addfirst() {
            this.teamOne+=2;
            if(this.teamOne >= this.scoreSet) {
                this.first = true
                this.showModal = true
                this.message = 'Team One won'
                this.mini = 'Sucks for Team Two'
                this.resetTimer()
            }
        },

        addfirst3() {
            this.teamOne+=3;
            if(this.teamOne >= this.scoreSet) {
                this.first = true
                this.showModal = !this.showModal
                this.message = 'Team One won'
                this.mini = 'Sucks for Team Two'
                this.resetTimer()
            }
        },

        removeFirst() {
            if(this.teamOne > 0) {
                this.teamOne-=2;
            }
        },

        removeFirst3() {
            if(this.teamOne > 0) {
                this.teamOne-=3;
            }
        },
        
        addSecond() {
            this.teamTwo+=3;
            if(this.teamTwo >= this.scoreSet) {
                this.sec = true
                this.showModal = !this.showModal
                this.message = 'Team Two won'
                this.mini = 'Sucks for Team One'
                this.resetTimer()
            }
        },

        addSecond2() {
            this.teamTwo+=2;
            if(this.teamTwo >= this.scoreSet) {
                this.sec = true
                this.showModal = !this.showModal
                this.message = 'Team Two won'
                this.mini = 'Sucks for Team One'
                this.resetTimer()
            }
        },

        removeSecond2() {
            if(this.teamOne > 0) {
                this.teamTwo-=2;
            }
            
        },

        removeSecond() {
            if(this.teamOne > 0) {
                this.teamTwo-=3;
            }
            
        },

        reset() {
            this.teamOne = 0
            this.teamTwo = 0
            this.scoreSet = 0
            this.resetTimer()
        },

        toggleStart() {
            this.gameOn = !this.gameOn;
            this.teamOne = 0
            this.teamTwo = 0
            this.first = false
            this.sec = false
        },

        toggleModal() {
            this.showModal = !this.showModal;
            this.first = !this.first
            this.sec = !this.sec
            this.toggleStart()
        },
    }
    
}
</script>

<style>

</style>