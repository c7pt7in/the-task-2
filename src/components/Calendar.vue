<template>
  <div class="calendar">
    <div class="header">
      <div class="prev" @click="showPrevMonth">
        <font-awesome-icon icon="chevron-left" />
      </div>
      <div class="title">
        {{ title }}
      </div>
      <div class="next" @click="showNextMonth">
        <font-awesome-icon icon="chevron-right" />
      </div>
    </div>
    <table>
      <thead>
        <tr>
          <th>mon</th>
          <th>tue</th>
          <th>wed</th>
          <th>thu</th>
          <th>fri</th>
          <th>sat</th>
          <th>sun</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(week, i ) in weeks" :key="i">
          <td v-for="weekday in week" :key="weekday.day"
            :class="{ 'first-of-month': weekday.day === 1, 'not-current-month': !weekday.isCurrentMonth }">
            <div v-if="weekday.day === 1" class="first-of-month-date">{{ weekday.month }}</div>
            {{ weekday.day }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  data: () => ({
    current: moment(),
  }),
  computed: {
    title() {
      return this.current.format('MMMM YYYY')
    },
    weeks() {
      const d = this.current.toDate()
      const currentMonth = d.getMonth()
      d.setDate(1)
      while (d.getDay() !== 1) {
        d.setDate(d.getDate() - 1)
      }
      let weeks = []
      for (let i = 0; i < 6; i++) {
        let week = []
        for (let j = 0; j < 7; j++) {
          week.push({
            day: d.getDate(),
            month: moment(d).format('MMM'),
            isCurrentMonth: d.getMonth() === currentMonth,
          })
          d.setDate(d.getDate() + 1)
        }
        weeks.push(week)
      }
      return weeks;
    }
  },
  methods: {
    showPrevMonth() {
      this.current = moment(this.current).subtract(1, 'M')
    },
    showNextMonth() {
      this.current = moment(this.current).add(1, 'M')
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.calendar {
  background: #f5f5f5;
  border: 1px solid #ccc;
  box-shadow: 5px 5px 20px 0 rgba(153, 153, 153, .5);
  width: 280px;
  margin: 0 auto;
}

.header {
  display: flex;
  height: 40px;
  line-height: 40px;;
}

.title {
  flex: 1;
  text-align: center;
  font-weight: 300;
  color: #777;
  font-size: 18px;
}

.prev,
.next {
  flex: 0 0 40px;
  text-align: center;
  color: #888;
  cursor: pointer;

  &:hover {
    background: #ddd;
    transition: all .5s ease;
  }
}

table {
  width: 100%;
}

thead {
  border-bottom: 1px solid #ccc;
}

th,
td {
  width: 40px;
}

th {
  font-weight: 300;
  color: #888;
  text-transform: uppercase;
  font-size: 12px;
  height: 20px;
  line-height: 20px;
}

td {
  height: 40px;
  color: #888;
  text-align: center;
}

.not-current-month {
  background: #eee;
}

.first-of-month {
  position: relative;
}

.first-of-month-date {
  position: absolute;
  top: 2px;
  width: 100%;
  text-align: center;
  font-size: 8px;
  font-weight: 300;
  color: #888;
  text-transform: uppercase;
}
</style>
