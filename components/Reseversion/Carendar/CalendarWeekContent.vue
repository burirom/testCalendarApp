<template>
  <div>
    <!-- <div v-if="!radios" class="d-flex carendar-box">
      <div class="calender-first-col">
        <div class="carendar-first-row d-flex align-center pa-2">
          <v-select :items="dorpdownList" label="Standard" dense></v-select>
          <v-btn icon disabled>
            <v-icon>mdi-cached</v-icon>
          </v-btn>
        </div>
        <div class="carendar-second-row no-item"></div>
        <CalendarTimeCol :times="times" :timedisplay="true" />
      </div>
      <div class="d-flex">
        <div
          v-for="(customerList, index) in customerLists"
          :key="`first-${index}`"
        >
          <ReservationBox
            v-if="DateComparison(dateItem, customerList.start)"
            :times="times"
            :business="Business"
            :customerinfo="customerList"
            :box-place-x="
              calcReservationBoxPlaceX(
                customerList.PersonInChargeId,
                PersonInChargeLists
              )
            "
          />
        </div>
        <div
          v-for="(PersonInChargeList, index) in PersonInChargeLists"
          :key="`second-${index}`"
          class="calender-col"
        >
          <div class="pa-2 carendar-first-row d-flex align-center">
            {{ PersonInChargeList.PersonInCharge }}
          </div>
          <div class="pa-2 carendar-second-row d-flex align-center">
            <p class="ma-0 second-row-text">ユニット表示</p>
            <v-btn class="second-row-btn" fab>
              <v-icon color="#c3c3c3">mdi-chevron-right</v-icon>
            </v-btn>
          </div>
          <CalendarTimeCol :times="times" />
        </div>
      </div>
      <div class="calender-last-col" :style="lastColWidth">
        <div class="pa-2 carendar-first-row"></div>
        <div class="pa-2 carendar-second-row no-item"></div>
        <CalendarTimeCol :times="times" />
      </div>
    </div> -->
    <CalendarCommon
      v-if="!radios"
      :top-row-contents="personInChargeLists"
      :times-text="times"
    >
      <template #secondrow>
        <p class="ma-0 second-row-text">ユニット表示</p>
      </template>
      <template #timeCol>
        <CalendarTimeCol :times="times" />
      </template>
    </CalendarCommon>
    <CalendarCommon
      v-else-if="radios"
      :top-row-contents="unitLists"
      :times-text="times"
      :col-width="150"
    >
      <template #secondrow>
        <div
          v-for="(personInChargeList,
          personInChargeIndex) in personInChargeLists"
          :key="personInChargeIndex"
          class="carendar-second-row-subcontent d-flex align-center justify-center"
        >
          <div>
            {{ personInChargeList.personInCharge.substring(0, 1) }}
          </div>
        </div>
      </template>
      <template #timeCol>
        <CalendarTimeCol :times="times" />
      </template>
    </CalendarCommon>
  </div>
</template>
<script>
export default {
  props: {
    dateItem: {
      default: null,
    },
    radios: {
      type: Boolean,
      default: true,
    },
  },
  data: () => ({
    dorpdownList: ['All', 'test1', 'test2'],
    Business: {
      openDate: new Date('2020/9/2/7:50'),
      closeDate: new Date('2020/9/2/17:0'),
    },
    times: [],
    personInChargeLists: [
      {
        id: 1,
        personInCharge: '担当者1',
      },
      {
        id: 2,
        personInCharge: '担当者2',
      },
      {
        id: 3,
        personInCharge: '担当者３',
      },
      {
        id: 4,
        personInCharge: '担当者４',
      },
      {
        id: 5,
        personInCharge: '担当者5',
      },
    ],
    unitLists: [
      {
        id: 1,
        unitName: 'ユニット1',
      },
      {
        id: 2,
        unitName: 'ユニット2',
      },
      {
        id: 3,
        unitName: 'ユニット３',
      },
      {
        id: 4,
        unitName: 'ユニット４',
      },
      {
        id: 5,
        unitName: 'ユニット5',
      },
    ],
    customerLists: [
      {
        start: new Date('2020/8/31/10:0'),
        end: new Date('2020/8/31/13:0'),
        name: '野村',
        customerNumberOfPeople: 0,
        nomination: false,
        PersonInChargeId: 4,
        unitId: 1,
      },
      {
        start: new Date('2020/9/2/10:0'),
        end: new Date('2020/9/2/14:0'),
        name: '野村',
        customerNumberOfPeople: 0,
        nomination: true,
        PersonInChargeId: 1,
        unitId: 2,
      },
    ],
  }),
  computed: {
    lastColWidth() {
      return {
        '---width':
          'calc(100% - ' +
          (this.PersonInChargeLists.length * 100 + 200) +
          'px)',
      }
    },
    numCol() {
      return {
        '---numcolwidth': 150 / this.PersonInChargeLists.length + 'px',
      }
    },
  },
  created() {
    this.setTimes()
  },
  methods: {
    calcBusinessTime(startDate, endDate) {
      const difftime = endDate.getHours() - startDate.getHours()
      return difftime
    },
    test() {
      this.calcReservationBoxPlaceX(this.customerLists[1].customerId)
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
    },
    DateComparison(dateItem, date) {
      if (!dateItem) return false
      if (dateItem.year !== date.getFullYear()) return false
      if (dateItem.month !== date.getMonth() + 1) return false
      if (dateItem.day !== date.getDate()) return false
      return true
    },
    calcReservationBoxPlaceX(Id, Lists) {
      const result = Lists.findIndex((object) => {
        return object.id === Id
      })
      if (result >= 0) return result
      return null
    },
  },
}
</script>
<style lang="scss" scoped>
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
.second-row-text {
  font-size: 11px;
  color: #a0a0a0;
  text-align: center;
}
.carendar-second-row {
  height: 45px;
  border-bottom: 1px solid #c3c3c3;
  background: #f5f6f8;
  &:first-child {
    border-top: 1px solid #c3c3c3;
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
.carendar-second-row-subcontent {
  height: 100%;
  border-right: 1px solid #c3c3c3;
  min-width: 30px;
  width: var(---numcolwidth);
  &:last-child {
    border-right: none;
  }
}
</style>
