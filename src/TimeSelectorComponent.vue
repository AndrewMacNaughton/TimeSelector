<template>
  <div class="start-time">
    <div class="pointer" @click="show">
      <div class="container wrapper" v-if="controlIsVisible">
        <div class="row ts">
          <div class="col-3">
            <div
              class="drop-down pointer"
              @click="showHours"
              :class="{ active: hoursIsVisible }"
            >
              {{ hours }} <i class="arrow down"></i>
              <div style="position: relative">
                <div class="control-wrapper">
                  <ul v-if="hoursIsVisible">
                    <li
                      @click.stop="updatedHours(item)"
                      v-for="item in hoursList"
                      :key="item"
                      :class="{ selected: item === hours }"
                    >
                      {{ item }}
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
          <div class="col-3">
            <div
              class="drop-down pointer"
              @click="showMinutes"
              :class="{ active: minutesIsVisible }"
            >
              {{ minutes }} <i class="arrow down"></i>
              <div style="position: relative">
                <div class="control-wrapper">
                  <ul v-if="minutesIsVisible">
                    <li
                      @click.stop="updatedMinutes(item)"
                      v-for="item in minutesList"
                      :key="item"
                      :class="{ selected: item === minutes }"
                    >
                      {{ item }}
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
          <div class="col-3">
            <div
              class="drop-down pointer"
              @click="showSeconds"
              :class="{ active: secondsIsVisible }"
            >
              {{ seconds }} <i class="arrow down"></i>
              <div style="position: relative">
                <div class="control-wrapper">
                  <ul v-if="secondsIsVisible">
                    <li
                      @click.stop="updatedSeconds(item)"
                      v-for="item in secondsList"
                      :key="item"
                      :class="{ selected: item === seconds }"
                    >
                      {{ item }}
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
          <div class="col-3">
            <button @click.stop="save()" class="stControl pointer">OK</button>
          </div>
        </div>
      </div>
      <div
        v-if="!showDisplayTime"
        class="slot-control"
        :class="{ active: controlIsVisible }"
      >
        {{ displayTime }}
      </div>
      <div v-else>
        <span class="material-icons smaller-icon">schedule</span>
      </div>
    </div>
    <div
      v-if="hoursIsVisible || minutesIsVisible || secondsIsVisible"
      class="outside"
      @click="handleClickAway('child')"
    ></div>
    <div
      v-if="controlIsVisible && !hoursIsVisible && !minutesIsVisible && !secondsIsVisible"
      class="outside"
      @click="handleClickAway('parent')"
    ></div>
  </div>
</template>

<script>
export default {
  name: 'StartTime',
  data() {
    return {
      controlIsVisible: false,
      hoursIsVisible: false,
      minutesIsVisible: false,
      secondsIsVisible: false,
      hours: '09',
      minutes: '00',
      seconds: '00',
      hoursList: [...Array(24).keys()].map((x) => this.addInitialZero(x)),
      minutesList: [...Array(60).keys()]
        .filter((x) => x % 5 === 0)
        .map((x) => this.addInitialZero(x)),
      secondsList: [...Array(60).keys()]
        .filter((x) => x % 5 === 0)
        .map((x) => this.addInitialZero(x)),
    }
  },
  computed: {
    displayTime() {
      return this.hours + ':' + this.minutes + ':' + this.seconds
    },
    showDisplayTime() {
      return (
        this.hours === '00' && this.minutes === '00' && this.seconds === '00'
      )
    },
  },
  methods: {
    addInitialZero(num) {
      return num.toString().padStart(2, '0')
    },
    updatedHours(item) {
      this.hours = item
      this.hoursIsVisible = false
    },
    updatedMinutes(item) {
      this.minutes = item
      this.minutesIsVisible = false
    },
    updatedSeconds(item) {
      this.seconds = item
      this.secondsIsVisible = false
    },
    controlUpdated(prop, item) {
      this.times[prop].value = item
      this.visibility.times[prop] = false
    },
    show() {
      this.controlIsVisible = true
      this.$emit('isActive')
    },
    showHours() {
      this.hoursIsVisible = true
    },
    showMinutes() {
      this.minutesIsVisible = true
    },
    showSeconds() {
      this.secondsIsVisible = true
    },
    handleClickAway(params) {
      if (params === 'parent') this.controlIsVisible = false
      if (params === 'child') this.hideAllChildControls()
    },
    hideAllChildControls() {
      this.secondsIsVisible = false
      this.minutesIsVisible = false
      this.hoursIsVisible = false
    },
    save() {
      this.$emit('saved', this.times)
      this.hideAllChildControls()
      this.controlIsVisible = false
    },
  },
}
</script>

<style lang='css' scoped>
.outside {
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0px;
  left: 0px;
  z-index: 0;
}
.start-time {
  display: inline-block;
}
.wrapper {
  position: relative;
}
.ts {
  position: absolute;
  top: -65px;
  right: 0px;
  background-color: white;
  padding: 8px 12px;
  border: solid 1px #ddd;
  border-radius: 4px;
  width:300px;
}
.seperator {
  padding: 12px 0px;
}
.stControl {
  padding: 8px 12px;
  margin: 4px;
  border-radius: 4px;
  background-color: #f9f9f9;
  border: solid 1px #ddd;
  width: 34px;
}
.slot-control {
  border-bottom: dashed 1px #bbb;
}
.slot-control.active {
  border-color: #666;
}
button {
  position: relative;
  z-index: 1;
  width: 48px !important;
}
.pointer {
  cursor: pointer;
}
.slide-enter-active,
.slide-leave-active {
  transition: height 0.5s ease-out;
}
.slide-enter,
.slide-leave-to {
  height: 0px;
}
.slide-enter-to,
.slide-leave {
  height: 300px;
}
div.control-wrapper {
  max-height: 300px;
  background-color: #f9f9f9;
  position: absolute;
  overflow-y: scroll;
  top: 8px;
  left: -13px;
}
ul {
  margin: 0px;
  width: 60px;
  margin-block-start: 0;
  padding-inline-start: 0;
  border: solid 1px #ddd;
  border-top: none;
  position: relative;
  z-index: 2;
}
li {
  list-style-type: none;
  padding: 4px;
  z-index: 2;
}
li.selected {
  background-color: #eee;
}
li:hover {
  background-color: #eee;
}
.arrow {
  border: solid #aaa;
  border-width: 0 2px 2px 0;
  display: inline-block;
  padding: 2px;
  margin-left: 5px;
  margin-bottom: 3px;
}
.arrow.down {
  transform: rotate(45deg);
}
.drop-down {
  padding: 8px 12px;
  margin: 4px;
  border-radius: 4px;
  background-color: #f9f9f9;
  border: solid 1px #ddd;
  width: 60px;
  position: relative;
  z-index: 2;
}
.drop-down.active {
  border-bottom-left-radius: 0px;
  border-bottom-right-radius: 0px;
  border-bottom: 0px;
}
</style>