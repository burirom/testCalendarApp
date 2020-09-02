<template>
  <div>
    <v-card>
      <CarendarHeader
        @sendDate="setDate"
        @sendRaiosData="setRaiosData"
        @sendCarendarList="setCarendarActiveList"
      />

      <CalendarDayContent
        v-if="activeCarendarList === 0"
        :date-item="dateItem"
        :radios="raiosData === 'unit' ? true : false"
      />
      <CalendarWeekContent
        v-else-if="activeCarendarList === 1"
        :radios="raiosData === 'unit' ? true : false"
      />
      <CalendarHalfMonthContent
        v-else-if="activeCarendarList === 2"
        :radios="raiosData === 'unit' ? true : false"
      />
    </v-card>
  </div>
</template>
<script>
export default {
  data: () => ({
    date: new Date(),
    dateItem: {
      year: null,
      month: null,
      day: null,
    },
    raiosData: 'unit',
    activeCarendarList: 0,
  }),
  computed: {
    testDate() {
      return this.date
    },
  },
  methods: {
    setDate(data) {
      this.dateItem.year = data.getFullYear()
      this.dateItem.month = data.getMonth() + 1
      this.dateItem.day = data.getDate()
    },
    setRaiosData(data) {
      this.raiosData = data
    },
    setCarendarActiveList(data) {
      this.activeCarendarList = data
    },
  },
}
</script>
