<template>
  <div class="calendar">
    <table>
      <thead>
        <tr>
          <th><a href="#" v-on:click="previousMonth">Prev</a></th>
          <th colspan="5"></th>
          <th><a href="#" v-on:click="nextMonth">Next</a></th>
        </tr>
        <tr>
          <th>Mon</th>
          <th>Tue</th>
          <th>Wed</th>
          <th>Thu</th>
          <th>Fri</th>
          <th>Sat</th>
          <th>Sun</th>
        </tr>
      </thead>
      <tbody data-bind="foreach:gridArray">
        <tr v-for="item in gridArray">
          <td v-for="data in item">
            {{data.getDate()}}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
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
.calendar table {
  border-collapse: collapse;
}

.calendar table th,
.calendar table td {
  text-align: center;
}
</style>
