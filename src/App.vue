<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        指定周波数の正弦波をスピーカーから出力
      </div>
    </v-app-bar>

    <v-main>
  <v-container>
    <v-row

      justify="space-between"
    >
      <v-col
        cols="12"
      >
        <p>
          <ul>
            <li>指定周波数の正弦波をスピーカから出力します。</li>
            <li>音楽理論を学習するうえで便利だと思ったので作成しました。</li>
            <li>スピーカーが壊れないように適度な音量・周波数でご活用ください。</li>
          </ul>
        </p>
         <v-card ref="form">
          <v-card-title>正弦波音</v-card-title>
          <v-card-text>
            <v-text-field
              v-model="frequency"
              type="number"
              label="周波数"
              suffix="Hz"
            ></v-text-field>
            <v-btn @click="doubleFreq">1オクターブ上げ</v-btn>
            <v-btn @click="halfFreq">1オクターブ下げ</v-btn>
          </v-card-text>

          <v-divider class="mt-12"></v-divider>

          <v-card-actions>
            <v-btn @click="play" :disabled="oscillator != null">出力開始</v-btn>
            <v-btn @click="stop" :disabled="oscillator == null">出力停止</v-btn>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>

    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'App',

  components: {
  },

  data: () => ({
    context: null,
    oscillator: null,
    frequency: 440
  }),

  created(){
    try {
      // Fix up for prefixing
      window.AudioContext = window.AudioContext||window.webkitAudioContext
      this.context = new AudioContext()
    }
    catch(e) {
      alert('Web Audio API is not supported in this browser');
    }
  },

  watch: {
    frequency(val){
      if (this.oscillator != null) {
        this.oscillator.frequency.value = val
      }
    }
  },

  methods: {
    play(){
      this.oscillator = new OscillatorNode(this.context)

      this.oscillator.frequency.value = this.frequency

      // オシレータの処理を開始
      this.oscillator.connect(this.context.destination)
      this.oscillator.start()
    },
    stop(){
      this.oscillator.stop()
      this.oscillator = null
    },
    doubleFreq(){
      this.frequency = this.frequency * 2
    },
    halfFreq(){
      this.frequency = this.frequency / 2
    }
  }
};
</script>
