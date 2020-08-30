<template>
  <div>
    <v-card>
      <CarendarHeader
        @sendDate="setDate"
        @sendRaiosData="setRaiosData"
        @sendCarendarList="setCarendarActiveList"
      />
      <ReservationBox />
      <CarendarDayContent />
      <!-- <CarendarWeekContent /> -->
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      date: new Date(),
      raiosData: 'unit',
      activeCarendarList: 0,
      damyData: [
        {
          title: 'ミーティング',
          start: new Date(),
          end: '2019-09-12T11:30:00',
          extendedProps: {
            department: '総務部',
          },
          description: '総務部とのミーティング',
        },
      ],
    }
  },
  methods: {
    setDate(date) {
      this.date = date
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

<style></style>
