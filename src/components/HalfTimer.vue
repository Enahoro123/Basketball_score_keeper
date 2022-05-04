<template>
  <div class="timer">
    <div class="left">
        <img src="../assets/wilder.png" alt="Wilders team logo">
    </div>
    <div class="middle">
        <div class="cont">

            <h1 class="lets"> Let's Ball Half-Court</h1>
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
                            <button @click="addfirst">+1</button> 
                            <button @click="addfirst3">+2</button>
                            <button @click="removeFirst">-1</button> 
                            <button @click="removeFirst3">-2</button>
                        </div>
                        
                    </div>
                        
                    <div class="player2">
                        
                        <h1>Team Two</h1>
                        <div class="buttons">
                            <button @click="addSecond2">+1</button>
                            <button @click="addSecond">+2</button>
                            <button @click="removeSecond2">-1</button>
                            <button @click="removeSecond">-2</button>
                        </div> 

                    </div>
                </div>
        </div>
            
    </div>            


    <div class="right">
        <img class="image2" src="../assets/Friendly.png" alt="Floating Head">
    </div>
      
  </div>    
  <div v-if="showModal">
        <Modal @close="toggleModal" theme="sale">
            <p class="score"><span :class="{ first }">{{ teamOne }}</span> : <span :class="{ sec }">{{ teamTwo }}</span> </p>
            <h1>{{ message }}</h1>
            <p>{{   mini    }}</p>
        </Modal></div>
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
            if(this.options[target].lengthInMinutes <= 99) {
                this.options[target].lengthInMinutes++
            }
            if(this.options[target].key == 0) {
                this.timer.duration.minutes++
            }
        },

        subTo(target) {
            if(this.options[target].lengthInMinutes > 0) {
                this.options[target].lengthInMinutes--
            }
            if(this.options[target].key == 0) {
                this.timer.duration.minutes--
            }
        },

        addfirst() {
            this.teamOne++;
            if(this.teamOne >= this.scoreSet) {
                this.first = true
                this.showModal = true
                this.message = 'Team One won'
                this.mini = 'Sucks for Team Two'
                this.resetTimer()
            }
            
        },
        addfirst3() {
            this.teamOne+=2;
            if(this.teamOne >= this.scoreSet) {
                this.first = true
                this.showModal = !this.showModal
                this.message = 'Team One won'
                this.mini = 'Sucks for for Team Two'
                this.resetTimer()
            }
            
        },
        removeFirst() {
            if(this.teamOne > 0) {
                this.teamOne--;
            }
        },
        removeFirst3() {
            if(this.teamOne > 0) {
                this.teamOne-=2;
            }
        },
        
        addSecond() {
            this.teamTwo+=2;
            if(this.teamTwo >= this.scoreSet) {
                this.sec = true
                this.showModal = !this.showModal
                this.message = 'Team Two won'
                this.mini = 'Sucks for Team One'
                this.resetTimer()
            }
        },
        addSecond2() {
            this.teamTwo++;
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
                this.teamTwo--;
            }
            
        },
        removeSecond() {
            if(this.teamOne > 0) {
                this.teamTwo-=2;
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
.cont {
   right: 60px;
   color: #fff;
   position: relative;
}
.timer {
    width: 100%;
    display: flex;
    justify-content: space-between;
}
img {
  width: 50%;
}
.image2 {
    width: 100%;
    margin: auto;
    bottom: 160px;
    position: relative;
}
.score {
    font-size: 70px;
    color: #bbbbbb;
    justify-content: space-between;
}
.lets {
    color: #fff;
    margin-bottom: 20px;
}
.first {
    color: green;
}
.sec {
    color: green;
}
input {
    margin: auto 4px;
}
.player1, .player2 {
    display: flex;
    max-width: 500px;
    color: #bbbbbb;
    margin: 20px auto;
    padding: 10px 20px;
    border-radius: 10px;
    align-items: center;
    list-style-type: none;
    background: rgb(30, 30, 30);
    justify-content: space-between;
    box-shadow: 1px 2px 3px rgba(50,50,50,0.05);
}


@media only screen and (max-width: 1027px) {
    img, .image2 {
        display:none;
    }
    .timer {
        display:block;
    }
    .middle{
        max-width: 100%;
    }
    .cont {
        right: 0px;
        position: relative;
    }
}
</style>