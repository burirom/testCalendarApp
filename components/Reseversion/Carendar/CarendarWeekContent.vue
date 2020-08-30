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
      <div class="d-flex justify-end">
        <h1>週間</h1>
        <CalendarTimeCol :times="times" :timedisplay="true" />
      </div>
    </div>
    <div
      v-for="(headerItemList, headerIndex) in headerItemLists"
      :key="headerIndex"
      class="calender-col"
    >
      <div class="pa-2 carendar-first-row d-flex align-center justify-center">
        {{ headerItemList.title }}
      </div>
      <div class="carendar-second-row d-flex">
        <div
          v-for="(personInCharge, personIndex) in personInCharges"
          :key="personIndex"
          class="carendar-second-row-subcontent d-flex align-center justify-center"
          :style="numCol"
        >
          <div>{{ personInCharge.person }}</div>
        </div>
      </div>
      <CalendarTimeCol :personcharges="personInCharges" :times="times" />
    </div>
    <!-- last col -->
    <div class="calender-last-col" :style="lastColWidth">
      <div class="pa-2 carendar-first-row"></div>
      <div class="pa-2 carendar-second-row no-item"></div>
      <CalendarTimeCol :times="times" />
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    dorpdownList: ['All', 'test1', 'test2'],
    times: [
      {
        text: '09:00',
      },
      {
        text: '10:00',
      },
      {
        text: '11:00',
      },
      {
        text: '12:00',
      },
      {
        text: '13:00',
      },
      {
        text: '14:00',
      },
      {
        text: '15:00',
      },
      {
        text: '16:00',
      },
      {
        text: '17:00',
      },
    ],
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
    ],
    personInCharges: [
      {
        person: '無',
      },
      {
        person: '吉',
      },
      {
        person: '田',
      },
      {
        person: '山',
      },
      {
        person: '山',
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
        '---numcolwidth': 150 / this.personInCharges.length + 'px',
      }
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
    right: -25%;
  }
  .carendar-second-row-subcontent {
    border-right: 1px solid #c3c3c3;
    min-width: 30px;
    width: var(---numcolwidth);
    &:last-child {
      border-right: none;
    }
  }
}

.calender-last-col {
  width: var(---width);
}

.calender-col {
  min-width: 150px;
  width: 150px;
  border-right: 1px solid #c3c3c3;
}

.calender-time-table {
  height: 20px;
  border-bottom: 1px dotted #e2e2e2;
  &:last-child {
    border-bottom: 1px solid #c3c3c3;
  }
}

.no-item {
  background: #fff;
}
</style>
