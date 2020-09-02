<template>
  <div>
    <CalendarCommon
      v-if="!radios"
      :top-row-contents="personInChargeLists"
      :times-text="times"
    >
      <template #reservationBox>
        <div
          v-for="(customerList, index) in customerLists"
          :key="`first-${index}`"
        >
          <ReservationBox
            v-if="DateComparison(dateItem, customerList.start)"
            :row-width="100"
            :business="Business"
            :customerinfo="customerList"
            :box-place-x="
              calcReservationBoxPlaceX(
                customerList.PersonInChargeId,
                personInChargeLists
              )
            "
            :box-place-y="
              calcReservationBoxPlaceY(Business.openDate, customerList.start)
            "
          />
        </div>
      </template>
      <template #secondrow>
        <p class="ma-0 second-row-text">ユニット表示</p>
      </template>
      <template #timeCol>
        <CalendarTimeCol :times="times" />
      </template>
      <template #secondrowbtn>
        <v-icon class="icon-btn" @click="changeUnitRadio">
          mdi-chevron-right
        </v-icon>
      </template>
    </CalendarCommon>
    <CalendarCommon
      v-else-if="radios"
      :top-row-contents="unitLists"
      :times-text="times"
      :col-width="colwidth"
    >
      <template #secondrow>
        <div
          v-for="(personInChargeList, personInChargeIndex) in personLists"
          :key="personInChargeIndex"
          class="carendar-second-row-subcontent d-flex align-center justify-center"
          :style="numCol"
        >
          <div>
            {{ personInChargeList.personInCharge.substring(0, 1) }}
          </div>
        </div>
      </template>
      <template #timeCol>
        <CalendarTimeCol
          :colwidth="colwidth"
          :personcharges="personLists"
          :times="times"
        />
      </template>
      <template #secondrowbtn>
        <v-icon class="icon-btn" @click="openpPerson">
          mdi-chevron-right
        </v-icon>
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
    colwidth: 200,
    personOpne: false,
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
      {
        id: 6,
        personInCharge: '担当者6',
      },
      {
        id: 7,
        personInCharge: '担当者6',
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
        start: new Date('2020/8/31/10:10'),
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
      if (!this.personOpne && this.personInChargeLists.length >= 6)
        return {
          '---numcolwidth': this.colwidth / 5 + 'px',
        }
      return {
        '---numcolwidth':
          this.colwidth / this.personInChargeLists.length + 'px',
      }
    },
    personLists() {
      if (!this.personOpne && this.personInChargeLists.length >= 6)
        return this.personInChargeLists.slice(0, 5)
      return this.personInChargeLists
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
    calcReservationBoxPlaceY(openDate, reservationDate) {
      const diffHours = Math.abs(
        openDate.getHours() - reservationDate.getHours()
      )
      const diffMinitu =
        ((diffHours * 60 + reservationDate.getMinutes()) / 10) * 20
      return diffMinitu
    },
    openpPerson() {
      if (this.personInChargeLists.length <= 5) return false
      this.personOpne = !this.personOpne
      if (this.personOpne) this.colwidth = this.personInChargeLists.length * 40
      if (!this.personOpne) this.colwidth = 200

      return false
    },
    changeUnitRadio() {
      this.radios = true
      console.log('押しました', this.radios)
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

.icon-btn {
  position: absolute;
  background: #f5f6f8;
  width: 22px;
  height: 22px;
  bottom: 0;
  right: 0;
  transform: translateY(-50%) translateX(50%);
  z-index: 9999;
  border-radius: 50%;
  border: 1px solid #c3c3c3;
}
</style>
