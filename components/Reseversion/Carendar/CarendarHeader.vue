<template>
  <div class="cal-top pa-8">
    <div class="nominate-mark">
      <div class="free">
        <span class="symbol"></span><span class="text">指名なし</span>
      </div>
      <div class="nominate">
        <span class="symbol"></span><span class="text">指名</span>
      </div>
    </div>
    <div class="next-before">
      <v-btn class="second-row-btn" fab @click="calendarLeftEvent()">
        <v-icon color="#c3c3c3">mdi-chevron-left</v-icon>
      </v-btn>
      <v-toolbar-title>{{ isDayDate }}</v-toolbar-title>
      <v-btn class="second-row-btn" fab @click="calendarRightEvent()">
        <v-icon color="#c3c3c3">mdi-chevron-right</v-icon>
      </v-btn>
    </div>
    <v-radio-group v-model="radios" :mandatory="false" row>
      <v-radio label="ユニット別" value="unit" @click="radiosEvent()"></v-radio>
      <v-radio label="担当者別" value="person" @click="radiosEvent()"></v-radio>
    </v-radio-group>
    <v-btn-toggle v-model="type" mandatory class="button-group">
      <v-btn
        v-for="item in list"
        :key="item.id"
        :value="item.value"
        small
        depressed
        :outlined="item.outlined"
        @click="carendarListBtnEvent(item.id)"
        >{{ item.label }}</v-btn
      >
    </v-btn-toggle>
  </div>
</template>
<script>
export default {
  data() {
    return {
      type: 0,
      list: [
        { id: 0, label: '日', outlined: false, value: 'day' },
        { id: 1, label: '週', outlined: true, value: 'week' },
        { id: 2, label: '半月', outlined: true, value: 'custom-weekly' },
        { id: 3, label: '月', outlined: true, value: 'month' },
      ],
      weeks: ['日', '月', '火', '水', '木', '金', '土'],
      radios: 'unit',
      date: new Date(),
      dateToString: '',
      weekAgoDateToString: '',
    }
  },
  computed: {
    isDayDate() {
      return this.dateToString
    },
    isRadios() {
      console.log(this.radios)
      return this.radios
    },
  },
  created() {
    this.setDateToString(this.date, this.dateToString)
    this.$emit('sendDate', this.date)
    console.log(this.date)
  },
  methods: {
    calendarLeftEvent() {
      this.changeDay(-1, this.date)
      this.setDateToString(this.date)
      this.$emit('sendDate', this.date)
      console.log(this.date)
    },
    calendarRightEvent() {
      this.changeDay(1, this.date)
      this.setDateToString(this.date)
      this.$emit('sendDate', this.date)
    },
    setDateToString(date) {
      this.dateToString = this.getDateToFromat(date)
    },
    setWeekAgoDateToString(date) {
      const weekagoDate = new Date(
        date.getFullYear(),
        date.getMonth() + 1,
        date.getDate() + 6
      )
      weekagoDate.setDate(weekagoDate.getDate())
      this.weekAgoDateToString = this.getDateToFromat(weekagoDate)
    },
    getDateToFromat(date) {
      const year = date.getFullYear()
      const month = date.getMonth() + 1
      const day = date.getDate()
      const week = this.weeks[date.getDay()]
      return year + '/' + month + '/' + day + '(' + week + ')'
    },
    changeDay(day, date) {
      date.setDate(date.getDate() + day)
    },
    changeMonth(month, date) {
      date.setMonth(date.getDate() + month)
    },
    changeYear(year, date) {
      date.setFullYear(date.getDate() + year)
    },
    radiosEvent() {
      this.$emit('sendRaiosData', this.radios)
    },
    carendarListBtnEvent(type) {
      this.$emit('sendCarendarList', type)
    },
  },
}
</script>
<style lang="scss" scoped>
.cal-top {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 33px;
  .next-before {
    position: relative;
    width: 230px;
    text-align: center;
    button {
      top: 0;
      bottom: 0;
      margin: auto;
      &:nth-child(1) {
        left: 0;
      }
      &:nth-child(2) {
        right: 0;
      }
    }
  }
  .nominate-mark {
    display: flex;
    & > div {
      display: flex;
      align-items: center;
      margin-right: 19px;
      .symbol {
        display: block;
        width: 66px;
        height: 20px;
        margin-right: 6px;
        border: 2px solid #6e9eff;
        border-radius: 3px;
        background: #e5eeff;
      }
      .text {
        color: #888;
      }
      &.nominate {
        .symbol {
          background: #6e9eff;
        }
      }
    }
  }
  .second-row-btn {
    box-shadow: none !important;
    width: 22px !important;
    height: 22px !important;
    border: 1px solid #c3c3c3;
  }
}
</style>
