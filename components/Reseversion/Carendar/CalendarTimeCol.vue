<template>
  <div style="width: 100%">
    <div v-if="isPersoncharges" class="d-flex">
      <div
        v-for="(personcharge, personIndex) in personcharges"
        :key="personIndex"
      >
        <div
          v-for="(time, index) in times"
          :key="index"
          class="carendar-time-col-personcharges"
          :style="numCol"
        >
          <p class="calender-time-table ma-0 pl-2"></p>
          <p class="calender-time-table ma-0 pl-2"></p>
          <p class="calender-time-table ma-0 pl-2"></p>
          <p class="calender-time-table ma-0 pl-2"></p>
          <p class="calender-time-table ma-0 pl-2"></p>
          <p class="calender-time-table ma-0 pl-2"></p>
        </div>
      </div>
    </div>
    <div v-else-if="timedisplay">
      <div v-for="(time, index) in times" :key="index">
        <p class="calender-time-table ma-0 pl-2 pr-2">{{ time.text }}</p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
      </div>
    </div>
    <div v-else>
      <div v-for="(time, index) in times" :key="index">
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
        <p class="calender-time-table ma-0 pl-2 pr-2"></p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    personcharges: {
      type: Array,
      required: false,
      default: null,
    },
    times: {
      type: Array,
      required: false,
      default: null,
    },
    timedisplay: {
      type: Boolean,
      required: false,
      default: false,
    },
    colwidth: {
      type: Number,
      required: false,
      default: 100,
    },
  },
  computed: {
    numCol() {
      const width = this.colwidth / this.personcharges.length
      console.log('幅', width)
      return {
        '---width': width + 'px',
      }
    },
    isPersoncharges() {
      if (this.personcharges === null) return false
      if (this.personcharges.length > 0) return true
      return false
    },
  },
}
</script>

<style lang="scss" scoped>
.carendar-time-col-personcharges {
  border-right: 1px solid #c3c3c3;
  width: var(---width);
}

.calender-time-table {
  height: 20px;
  border-bottom: 1px dotted #e2e2e2;
  &:last-child {
    border-bottom: 1px solid #c3c3c3;
  }
}
</style>
