<template>
  <div id="app">
    <h1>London to Amsterdam Countdown</h1>
    <h2>
      {{ countDown }}
    </h2>
    <h3>
      (That means {{ getNumberOfSundays() }})
    </h3>
  </div>
</template>

<script>
import TimeAgo from 'javascript-time-ago'
import en from 'javascript-time-ago/locale/en'
TimeAgo.addDefaultLocale(en)
const timeAgo = new TimeAgo('en-GB')

export default {
  name: 'App',
  data() {
    return {
      countDown: 0,
      l2a: new Date('2022-04-02')
    }
  },
  created() {
    this.countDown = this.toGo()
    setInterval(() => {
      this.countDown = this.toGo()
    }, 1000)
  },
  methods: {
    toGo() {
      const now = new Date()

      let delta = Math.abs(this.l2a - now) / 1000

      let days = Math.floor(delta / 86400)
      const weeks = Math.floor(days / 7)
      days -= weeks * 7
      delta -= days * 86400

      const hours = Math.floor(delta / 3600) % 24
      delta -= hours * 3600

      const minutes = Math.floor(delta / 60) % 60
      delta -= minutes * 60

      const seconds = Math.floor(delta % 60)

      return [
        this.format(weeks, 'week'),
        this.format(days, 'day'),
        this.format(hours, 'hour'),
        this.format(minutes, 'minute'),
        this.format(seconds, 'second')
      ].join(', ')
    },
    getNumberOfSundays() {
      const now = new Date()
      const sundays = this.getNumberOfWeekDays(now, this.l2a, 0)
      const label = sundays == 1 ? 'Sunday' : 'Sundays'
      return sundays + ' ' + label + '!'
    },
    getNumberOfWeekDays(start, end, dayNum){
      // Sunday's num is 0 with Date.prototype.getDay
      dayNum = dayNum || 0
      // Calculate the number of days between start and end.
      const daysInInterval = Math.ceil((end.getTime() - start.getTime()) / (1000 * 3600 * 24))
      // Calculate the nb of days before the next target day (e.g. next Sunday after start)
      const toNextTargetDay = (7 + dayNum - start.getDay()) % 7
      // Calculate the number of days from the first target day to the end.
      const daysFromFirstTargetDay = Math.max(daysInInterval - toNextTargetDay, 0)
      // Calculate the number of weeks (even partial) from the first target day to the end
      return Math.ceil(daysFromFirstTargetDay / 7)
    },
    format(n, unit) {
      const label = n == 1 ? unit : unit + 's'
      return n + ' ' + label
    }

  }
}
</script>

<style lang="scss">

  h1 {
    margin-top: 24px;
    font-size: 4.0rem;
  }

  h2 {
    margin-top: 24px;
    font-size: 3.5rem;
  }

  h3 {
    font-weight: bold;
    font-style: italic;
    margin-top: 24px;
    color: darkred;
  }

  @media only screen and (max-device-width: 666px) {
    h1 {
      font-size: 3rem;
    }

    h2 {
      font-size: 2rem;
    }
  }

  @media only screen and (max-device-width: 650px) {
    h1 {
      font-size: 2rem;
    }

    h2 {
      font-size: 1.75rem;
    }
  }

</style>
