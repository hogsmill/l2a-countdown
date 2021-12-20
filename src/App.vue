<template>
  <div id="app">
    <h1>London to Amsterdam Countdown</h1>
    <h2>
      {{ countDown }}
    </h2>
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
      countDown: 0
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
      const l2a = new Date('2022-04-02')
      const now = new Date()

      let delta = Math.abs(l2a - now) / 1000

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
</style>
