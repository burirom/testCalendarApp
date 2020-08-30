<template>
  <div>
    <v-btn @click="setIntDiffMinute()">テストボタン</v-btn>
    <div
      :style="boxHeight"
      class="reservation-box"
      :class="nomination ? 'nomination-box' : 'no-nomination-box'"
    ></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: 'ミーティング',
      start: new Date(),
      end: null,
      extendedProps: {
        department: '総務部',
      },
      description: '総務部とのミーティング',
      intDiffMinute: 0,
      nomination: false,
    }
  },
  computed: {
    boxHeight() {
      return {
        '---height': this.calcBoxheight(this.intDiffMinute) + 'px',
      }
    },
  },
  created() {
    const date = new Date(
      this.start.getFullYear(),
      this.start.getMonth(),
      this.start.getDate(),
      this.start.getHours(),
      this.start.getMinutes() + 90
    )
    this.end = date
  },
  methods: {
    setIntDiffMinute() {
      this.intDiffMinute = this.calcDiffTime()
    },
    // 時間の差分の計算(分)
    calcDiffTime() {
      const diff = this.start.getTime() - this.end.getTime()
      const diffMinute = Math.abs(diff) / (60 * 1000)
      return Math.round(diffMinute)
    },
    // 差分の時間いよるboxの高さの計算
    calcBoxheight(diiftime) {
      const Boxheight = Math.round(diiftime / 10) * 20
      return Boxheight
    },
  },
}
</script>

<style lang="scss" scoped>
.reservation-box {
  width: 100px;
  height: var(---height);
}
.nomination-box {
  background: #6e9eff;
  border: 1px solid #6e9eff;
  border-radius: 3px;
}
.no-nomination-box {
  background: #e5eeff;
  border: 2px solid #6e9eff;
  border-radius: 3px;
}
</style>
