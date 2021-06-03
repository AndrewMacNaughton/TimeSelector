
<script>
export default {
  render(h) {
    return this.$scopedSlots.default({
      times: this.includedTimes,
      items: this.dynamicItems,
      visibility: this.visibility,
      showChild: this.handleChildShown,
      updated: this.handleControlUpdated,
      clickAwayParent: () => this.handleClickAway('parent'),
      clickAwayControls: () => this.handleClickAway('child'),
      displayTime: this.displayTime,
      showControls: this.show,
      close: this.save,
    })
  },

  name: 'StartTime',
  created() {
    this.HOURS = 'hours'
    this.MINUTES = 'minutes'
    this.SECONDS = 'seconds'
    if (this.includeSeconds === true && !this.selectedTime.seconds) {
      this.selectedTime.seconds = '00'
    }
    this.times = Object.keys(this.selectedTime).reduce((acc, key) => {
      acc[key] = {
        value: this.selectedTime[key],
        include: true,
      }
      return acc
    }, {})
  },
  data() {
    return {
      visibility: {
        controls: false,
        times: {
          minutes: false,
          hours: false,
          seconds: false,
        },
      },
      times: {},
    }
  },
  props: {
    hours: {
      type: Array,
      default: () => [...Array(24).keys()].map((x) => addInitialZero(x)),
    },
    minutes: {
      type: Array,
      default: () => defaultMinsAndSecs(),
    },
    seconds: {
      type: Array,
      default: () => defaultMinsAndSecs(),
    },
    includeSeconds: {
      type: Boolean,
      default: () => false,
    },
    seperator: {
      type: String,
      default: () => ':',
    },
    selectedTime: {
      type: Object,
      validator: (value) => {
        return true
      },
      default: () => {
        return {
          hours: '00',
          minutes: '00',
        }
      },
    },
  },
  computed: {
    includedTimes() {
      return Object.keys(this.times)
        .map((key) => {
          return { ...this.times[key], key }
        })
        .filter((time) => time.include === true)
    },
    displayTime() {
      if (
        Object.keys(this.times).every((key) => this.times[key].value === '00')
      ) {
        return null
      }
      const timeWithoutSeconds =
        this.times.hours.value + this.seperator + this.times.minutes.value
      return this.includeSeconds
        ? timeWithoutSeconds + this.seperator + this.times.seconds.value
        : timeWithoutSeconds
    },
  },
  methods: {
    dynamicItems(key) {
      return this[key]
    },
    handleControlUpdated(prop, item) {
      this.times[prop].value = item
      this.visibility.times[prop] = false
    },
    show() {
      this.visibility.controls = true
      this.$emit('isActive')
    },
    handleChildShown(item) {
      if (this.visibility.times[item] === true) {
        this.hideAllChildControls()
      } else {
        this.hideAllChildControls()
        this.visibility.times[item] = true
      }
    },
    handleClickAway(params) {
      if (params === 'parent') this.visibility.controls = false
      if (params === 'child') this.hideAllChildControls()
    },
    hideAllChildControls() {
      Object.keys(this.visibility.times).forEach(
        (key) => (this.visibility.times[key] = false)
      )
    },
    save() {
      this.$emit('saved', this.times)
      this.hideAllChildControls()
      this.visibility.controls = false
    },
  },
}

const addInitialZero = (num) => {
  return num.toString().padStart(2, '0')
}
const defaultMinsAndSecs = () =>
  [...Array(60).keys()].filter((x) => x % 5 === 0).map((x) => addInitialZero(x))
</script>