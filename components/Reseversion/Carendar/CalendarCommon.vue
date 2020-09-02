<template>
  <div class="carendar-box">
    <div class="calender-header d-flex">
      <div
        class="carendar-first-row d-flex align-center pa-20 calender-first-col first-content justify-center"
      >
        <slot name="firstitem"></slot>
      </div>
      <div
        v-for="n of colNum"
        :key="n"
        class="carendar-first-row d-flex align-center pa-20 calender-col justify-center col-width"
        :style="numWidth"
      >
        <slot name="firstrow"></slot>
      </div>
    </div>
    <div class="calender-header d-flex">
      <div
        class="d-flex align-center calender-header-content pa-20 justify-center first-content border-right"
      ></div>
      <div
        v-for="n of colNum"
        :key="n"
        class="d-flex align-center pa-20 calender-col justify-center col-width"
        :style="numWidth"
      >
        <slot name="secondrow"></slot>
      </div>
    </div>
    <!-- CarendarHeader -->
    <div class="d-flex">
      <div class="d-flex justify-end first-content border-right">
        <slot name="firstColContent"></slot>
        <CalendarTimeCol :times="timesText" :timedisplay="true" />
      </div>
      <div
        v-for="n of colNum"
        :key="n"
        class="border-right col-width d-flex"
        :style="numWidth"
      >
        <slot name="timeCol"></slot>
        <CalendarTimeCol :times="timesText" />
      </div>
      <div class="calender-last-col" :style="lastColWidth">
        <CalendarTimeCol :times="timesText" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    topRowContents: {
      type: Array,
      default: null,
    },
    secondRowContents: {
      type: Array,
      default: null,
    },
    timesText: {
      default: null,
    },
    colNum: {
      type: Number,
      default: 6,
    },
    colWidth: {
      type: Number,
      default: 100,
    },
  },
  data: () => ({
    dorpdownList: ['All', 'test1', 'test2'],
  }),
  computed: {
    lastColWidth() {
      return {
        '---width':
          'calc(100% - ' + this.colNum * this.colWidth + 'px + ' + 200 + 'px)',
      }
    },
    numCol() {
      return {
        '---numcolwidth': 150 / this.secondRowContents.length + 'px',
      }
    },
    numWidth() {
      return {
        '---width': this.colWidth + 'px',
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.carendar-box {
  overflow: auto;
}
.calender-header {
  border-bottom: 1px solid #c3c3c3;
}
.calender-header-content {
  height: 43px;
}

.first-content {
  width: 200px;
  min-width: 200px;
}
.calender-first-col {
  width: 200px;
  min-width: 200px;
  border-right: 1px solid #c3c3c3;
}
.carendar-first-row {
  height: 62px;
  background: #f5f6f8;
}
.carendar-text {
  text-align: center;
}

.border-right {
  border-right: 1px solid #c3c3c3;
  &:last-child {
    border-right: none;
  }
}

.carendar-second-row {
  height: 45px;
  background: #f5f6f8;
  .second-row-text {
    font-size: 11px;
    color: #a0a0a0;
    text-align: center;
  }
  .second-row-btn {
    box-shadow: none !important;
    width: 22px !important;
    height: 22px !important;
    position: relative;
    right: -25%;
  }
  .carendar-second-row-subcontent {
    border-right: 1px solid #c3c3c3;
    min-width: 30px;
    width: 100%;
    &:last-child {
      border-right: none;
    }
  }
}
.col-width {
  width: var(---width);
  min-width: var(---width);
}
.calender-last-col {
  width: var(---width);
}
.calender-col {
  border-right: 1px solid #c3c3c3;
}

.no-item {
  background: #fff;
}
</style>
