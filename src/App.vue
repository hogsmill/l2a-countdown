<template>
  <div id="app">
    <h1>L2A Countdown</h1>
    <div>
      {{ countDown }}
    </div>
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

      const days = Math.floor(delta / 86400)
      delta -= days * 86400

      const hours = Math.floor(delta / 3600) % 24
      delta -= hours * 3600

      const minutes = Math.floor(delta / 60) % 60
      delta -= minutes * 60

      const seconds = Math.floor(delta % 60)

      return [
        this.format(days, 'day'),
        this.format(hours, 'hours'),
        this.format(minutes, 'minute'),
        this.format(seconds, 'second')
      ].join(', ')
    },
    format(n, unit) {
      const label = n < 10 ? unit : unit + 's'
      return n + ' ' + label
    }

  }
}
</script>

<style lang="scss">

  h1 {
    margin-top: 24px;
  }
</style>
