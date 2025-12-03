<template>
  <div>
    <h2>Enter last draw numbers</h2>
    <input v-for="i in 6" :key="i" v-model.number="lastDraw[i-1]" type="number" min="1" max="40"/>
    <h3>Optional numbers</h3>
    <input v-for="i in 5" :key="i" v-model.number="userNumbers[i-1]" type="number" min="1" max="40"/>
    <button @click="getPrediction">Predict</button>
    <div v-if="top5.length"><h3>Top 5 per slot:</h3><div v-for="(slot, i) in top5" :key="i">{{ slot.join(', ') }}</div></div>
    <div v-if="sets.length"><h3>Predicted sets:</h3><div v-for="(s, i) in sets" :key="i">{{ s.join(', ') }}</div></div>
    <div class="adsense">
      <ins class="adsbygoogle" style="display:block" data-ad-client='ca-pub-XXXXXXXXXX' data-ad-slot='YYYYYYYYYY' data-ad-format='auto' data-full-width-responsive='true'></ins>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return { lastDraw: Array(6).fill(1), userNumbers: Array(5).fill(1), top5: [], sets: [] }
  },
  methods: {
    async getPrediction() {
      const res = await axios.post('https://lotto-backend-mwts.onrender.com/predict', { last_draw: this.lastDraw, user_numbers: this.userNumbers })
      this.top5 = res.data.top5_per_slot
      this.sets = res.data.predicted_sets
      (window.adsbygoogle = window.adsbygoogle || []).push({})
    }
  }
}
</script>
