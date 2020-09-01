```
<template>
  <div class="d-flex carendar-box">
    <div class="calender-first-col">
      <!-- first col -->
      <div class="carendar-first-row d-flex align-center pa-2">
        <v-select :items="dorpdownList" label="Standard" dense></v-select>
        <v-btn icon disabled>
          <v-icon>mdi-cached</v-icon>
        </v-btn>
      </div>
      <div class="carendar-second-row no-item"></div>
      <!-- time col -->
      <CalendarTimeCol :times="times" :timedisplay="true" />
    </div>
    <div class="d-flex">
      <!-- ReservationBox -->
      <ReservationBox :times="times" :business="Business" />
      <div
        v-for="(headerItemList, headerIndex) in headerItemLists"
        :key="headerIndex"
        class="calender-col"
      >
        <div class="pa-2 carendar-first-row d-flex align-center">
          {{ headerItemList.title }}
        </div>
        <div class="pa-2 carendar-second-row d-flex align-center">
          <p class="ma-0 second-row-text">ユニット表示</p>
          <v-btn class="second-row-btn" fab>
            <v-icon color="#c3c3c3">mdi-chevron-right</v-icon>
          </v-btn>
        </div>
        <!-- time col -->
        <CalendarTimeCol :times="times" />
      </div>
    </div>
    <!-- last col -->
    <div class="calender-last-col" :style="lastColWidth">
      <div class="pa-2 carendar-first-row"></div>
      <div class="pa-2 carendar-second-row no-item"></div>
      <!-- time col -->
      <CalendarTimeCol :times="times" />
    </div>
  </div>
</template>
<script>
export default {
  data: () => ({
    dorpdownList: ['All', 'test1', 'test2'],
    Business: {
      openDate: new Date(2020, 8, 31, 7, 50),
      closeDate: new Date(2020, 8, 31, 17, 0),
    },
    times: [],
    headerItemLists: [
      {
        title: 'ユーザー1',
      },
      {
        title: 'ユーザー2',
      },
      {
        title: 'ユーザー３',
      },
      {
        title: 'ユーザー４',
      },
      {
        title: 'ユーザー5',
      },
    ],
  }),
  computed: {
    lastColWidth() {
      return {
        '---width':
          'calc(100% - ' + (this.headerItemLists.length * 100 + 200) + 'px)',
      }
    },
    numCol() {
      return {
        '---numcolwidth': 150 / this.personcharges.length + 'px',
      }
    },
  },
  created() {
    this.setTimes()
  },
  methods: {
    calcBusinessTime(startDate, endDate) {
      const difftime = endDate.getTime() - startDate.getTime()
      const diffHour = Math.floor(difftime / (1000 * 60 * 60))
      return diffHour
    },
    test() {
      this.setTimes(this.Business.openDate, this.Business.closeDate)
    },
    setTimes() {
      this.times = []
      const businesstTime = this.calcBusinessTime(
        this.Business.openDate,
        this.Business.closeDate
      )
      const startHour = this.Business.openDate.getHours()
      for (let i = 0; i <= businesstTime; i++) {
        this.times.push({
          text: startHour + i + ':00',
        })
      }
      console.log(this.times)
    },
  },
}
</script>
<style lang="scss">
.carendar-box {
  overflow: auto;
}
.first-content {
  width: 200px;
}
.calender-first-col {
  width: 200px;
  min-width: 200px;
  border-right: 1px solid #c3c3c3;
}
.calender-col {
  min-width: 100px;
  width: 100px;
  border-right: 1px solid #c3c3c3;
}
.calender-time-table {
  height: 20px;
  border-bottom: 1px dotted #e2e2e2;
  &:last-child {
    border-bottom: 1px solid #c3c3c3;
  }
}

.calender-last-col {
  width: var(---width);
}

.carendar-first-row {
  height: 62px;
  border-bottom: 1px solid #c3c3c3;
  background: #f5f6f8;
  &:first-child {
    border-top: 1px solid #c3c3c3;
  }
}
.carendar-second-row {
  height: 45px;
  border-bottom: 1px solid #c3c3c3;
  background: #f5f6f8;
  &:first-child {
    border-top: 1px solid #c3c3c3;
  }
  .second-row-text {
    font-size: 11px;
    color: #a0a0a0;
    text-align: center;
  }
  .second-row-btn {
    box-shadow: none !important;
    width: 22px !important;
    height: 22px !important;
    border: 1px solid #c3c3c3;
    position: relative;
    right: -20%;
  }
}
.no-item {
  background: #fff;
}
</style>
