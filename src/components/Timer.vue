<template>
  <div class="full-width text-center">
    <div class="text-center text-grey-2 full-width  q-pa-sm">
      Choose your time:
      <q-option-group v-model="timeChoose" :options="timeOptions" color="grey-1" dark inline/>
    </div>
    <q-circular-progress
      show-value
      class="text-white  q-mt-lg q-mb-lg justify-center"
      :max="initialTime"
      :value="totalTime"
      size="230px"
      :thickness="0.13"
      :color="timerColor"
      track-color="dark"
    >

      <div class="column justify-center items-center">

        <span class="q-pt-lg q-mt-lg" :class="`text-${timerColor}`">{{minutes}}:{{seconds}}</span>
        <q-btn
          flat
          size="15px"
          :color="timerColor"
          class="q-mt-md "
          stack
          icon="settings"
          @click="sliders = true"
        >
        </q-btn>
        <q-dialog v-model="sliders" >
          <q-card style="width: 300px" class="q-px-sm q-pb-md  ">
            <q-card-section>
              <div class="text-h6">Settings</div>
            </q-card-section>

            <q-item-label header>Pomodoro</q-item-label>
            <q-item dense>
              <q-item-section avatar>
                <q-icon name="watch_later" />
              </q-item-section>
              <q-item-section>
                <q-badge color="blue-grey-9" class="q-mr-auto" rounded>{{timeOptions[0].value}}</q-badge>
                <q-slider color="blue-grey-9" v-model="timeOptions[0].value" :step="1" :min="0" :max="99"/>
              </q-item-section>
            </q-item>

            <q-item-label header>Short Break</q-item-label>
            <q-item dense>
              <q-item-section avatar>
                <q-icon name="alarm" />
              </q-item-section>
              <q-item-section>
                <q-badge color="blue-grey-9" class="q-mr-auto" :label="timeOptions[1].value" rounded></q-badge>
                <q-slider color="blue-grey-9" v-model="timeOptions[1].value" :step="1" :min="0" :max="15"/>
              </q-item-section>
            </q-item>

            <q-item-label header>Long Break</q-item-label>
            <q-item dense>
              <q-item-section avatar>
                <q-icon name="alarm" />
              </q-item-section>
              <q-item-section>
                <q-badge color="blue-grey-9" class="q-mr-auto" rounded>{{timeOptions[2].value}}</q-badge>
                <q-slider color="blue-grey-9" v-model="timeOptions[2].value" :step="1" :min="0" :max="30 "/>
              </q-item-section>
            </q-item>
          </q-card>
        </q-dialog>
      </div>


    </q-circular-progress>
    <div class="row justify-center q-mt-none q-mb-lg full-width">
      <q-btn
        flat
        v-if="!pauseButton"
        size="15px"
        color="white"
        label="Start"
        stack
        icon="play_arrow"
        @click="startTimer"
      ></q-btn>
      <q-btn
        flat
        size="15px"
        color="white"
        label="Pause"
        @click="pauseTimer"
        v-if="pauseButton"
        stack
        icon="pause"
      ></q-btn>
      <q-btn
        v-show="resetButton"
        flat
        size="15px"
        color="white"
        label="Reset"
        @click="resetTimer"
        stack
        icon="restore"
      ></q-btn>
    </div>
  </div>
</template>
<script>
import BrowserNotifications from '../mixins/BrowserNotifications'
export default {
  mixins: [BrowserNotifications],
  data () {
    return {
      timer: null,
      timeChoose: 25,
      pauseButton: false,
      resetButton: false,
      totalTime: 25 * 60,
      timeOptions: [{ label: 'Pomodoro', value: 25 }, { label: 'Short Break', value: 5 }, { label: 'Long Break', value: 10 }],
      sliders: false,
    }
  },
  computed: {
    initialTime () {
      return this.timeChoose * 60
    },
    minutes () {
      const minutes = Math.floor(this.totalTime / 60)
      return this.padTime(minutes)
    },
    seconds () {
      const seconds = this.totalTime - (this.minutes * 60)
      return this.padTime(seconds)
    },
    timerColor () {
      const value = this.totalTime / 60
      return value <= 3 ? 'red-6' : value <= 10 ? 'orange-6' : 'white'
    }
  },
  watch: {
    timeChoose () {
      this.resetTimer()
    },
    totalTime () {
      if (this.totalTime === 0) {
        this.showNotification('Time is up!', '', '../../public/54589233.jpg')
      }
    }
  },
  meta () {
    return {
      title: this.timer ? `(${this.minutes}:${this.seconds}) Pomodoro Timer` : 'Pomodoro Timer'
    }
  },
  methods: {
    startTimer () {
      this.timer = setInterval(() => this.countdown(), 1000)
      this.resetButton = true
      this.pauseButton = true
    },
    pauseTimer () {
      this.pauseButton = false
      clearInterval(this.timer)
      this.timer = null
      this.resetButton = true
    },
    resetTimer () {
      this.totalTime = (this.timeChoose * 60)
      clearInterval(this.timer)
      this.timer = null
      this.resetButton = false
      this.pauseButton = false
    },
    padTime (time) {
      return (time < 10 ? '0' : '') + time
    },
    countdown () {
      if (this.totalTime >= 1) {
        this.totalTime--
      } else {
        this.totalTime = 0
        this.resetTimer()
      }
    }
  }
}
</script>
