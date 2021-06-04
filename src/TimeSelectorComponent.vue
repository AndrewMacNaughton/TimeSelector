
<script>
export default {
  render() {
    return this.$scopedSlots.default({
      times: this.includedTimes,
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

  name: 'TimeSelector',
  created() {
    console.log(this.$slots)
    if (this.includeSeconds === true && !this.selectedTime.seconds) {
      this.selectedTime.seconds = '00'
    }
    this.times = Object.keys(this.selectedTime).reduce((acc, key) => {
      acc[key] = {
        value: this.selectedTime[key],
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
      default: () => {
        return {
          hours: '00',
          minutes: '00',
        }
      },
    },
  },
  computed: {
    // Exported as the times slot.
    includedTimes() {
      return Object.keys(this.times)
        .map((key) => {
          // Add the key (eg. 'hours' as a property on the object)
          return { ...this.times[key], key }
        })
        .map((time) => {
          // Add the items to the control for easy access in the view
          return {
            ...time,
            items: this[time.key],
          }
        })
    },
    displayTime() {
      const timeWithoutSeconds =
        this.times.hours.value + this.seperator + this.times.minutes.value
      return this.includeSeconds
        ? timeWithoutSeconds + this.seperator + this.times.seconds.value
        : timeWithoutSeconds
    },
  },
  methods: {
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