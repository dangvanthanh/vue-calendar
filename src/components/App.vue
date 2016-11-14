<template>
  <div class="calendar">
    <div class="calendar-container">
      <div class="calendar-toolbar">
        <button @click="previousMonth">Prev</button>
        <div class="calendar-toolbar-title"></div>
        <button @click="nextMonth">Next</button>
      </div>
      <div class="calendar-week">
        <span v-for="week in weeks">{{week}}</span>
      </div>
      <div class="calendar-monthday">
        <div class="calendar-slide">
          <div class="calendar-content" data-bind="foreach:gridArray">
            <div class="calendar-row" v-for="monthday in gridArray">
              <div class="calendar-cell" v-for="day in monthday">{{day.getDate()}}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- <table>
      <thead>
        <tr>
          <th><a href="#" v-on:click="previousMonth">Prev</a></th>
          <th colspan="5"></th>
          <th><a href="#" v-on:click="nextMonth">Next</a></th>
        </tr>
        <tr>
          <th v-for="week in weeks">
            {{week}}
          </th>
        </tr>
      </thead>
      <tbody data-bind="foreach:gridArray">
        <tr v-for="item in gridArray">
          <td v-for="data in item">
            {{data.getDate()}}
          </td>
        </tr>
      </tbody>
    </table> -->
  </div>
</template>

<script>
export default {
  props: {
    weeks: {
      type: Array,
      default: () => {
        return ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
      }
    }
  },
  data () {
    return {
      selectedMonth: new Date()
    }
  },
  computed: {
    gridArray () {
      let grid = this.getCalendar(this.selectedMonth)
      return grid
    }
  },
  methods: {
    getCalendar (date) {
      let calendar = []
      let startDay = new Date(date.getFullYear(), date.getMonth(), 1)
      let lastDay = new Date(date.getFullYear(), date.getMonth() + 1, 0)

      // Modify the result of getDay so that we treat Monday = 0 instead of Sunday = 0
      let startDow = (startDay.getDay() + 6) % 7
      let endDow = (lastDay.getDay() + 6) % 7

      // If the month didn't start on a Monday, start from the last Monday of the previous month
      startDay.setDate(startDay.getDate() - startDow)

      // If the month didn't end on a Sunday, end on the following Sunday in the next month
      lastDay.setDate(lastDay.getDate() + (6 - endDow))

      let week = []

      while (startDay <= lastDay) {
        week.push(new Date(startDay))

        if (week.length === 7) {
          calendar.push(week)
          week = []
        }

        startDay.setDate(startDay.getDate() + 1)
      }

      return calendar
    },
    previousMonth () {
      let tmpDate = this.selectedMonth
      let tmpMonth = tmpDate.getMonth() - 1
      this.selectedMonth = new Date(tmpDate.setMonth(tmpMonth))
    },
    nextMonth () {
      let tmpDate = this.selectedMonth
      let tmpMonth = tmpDate.getMonth() + 1
      this.selectedMonth = new Date(tmpDate.setMonth(tmpMonth))
    }
  }
}
</script>

<style>
.calendar {
  width: 310px;
  display: flex;
  flex-direction: column;
}

.calendar-container {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
}

.calendar-toolbar {
  display: flex;
  justify-content: space-between;
}

.calendar-week {
  display: flex;
  justify-content: space-between;
  flex-direction: row;
}

.calendar-row {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

.calendar-cell {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
</style>
