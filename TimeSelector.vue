<template>
  <time-selector-component :selectedTime="selectedTime">
    <div
      class="start-time"
      slot-scope="{
        times,
        items,
        visibility,
        showChild,
        updated,
        displayTime,
        showControls,
        clickAwayParent,
        clickAwayControls,
        close,
      }"
    >
      <transition name="show">
        <div class="container-wrapper" v-if="visibility.controls">
          <div class="grid-container">
            <time-selector-control
              class="stControl pointer"
              v-for="(item, index) in times"
              :key="index"
              :items="items([item.key])"
              :selected="item.value"
              :isVisible="visibility.times[item.key]"
              @showChild="showChild(item.key)"
              @updated="updated(item.key, $event)"
              >{{ item.value }}
            </time-selector-control>
            <button @click.stop="close" class="stControl pointer">OK</button>
          </div>
        </div>
      </transition>
      <div
        @click="showControls"
        class="slot-control-wrapper"
        :class="{ active: visibility.controls }"
      >
        <div class="slot-control pointer">
          <span class="dashed-border" v-if="displayTime">{{ displayTime }}</span>
          <span class="dashed-border" v-else>
            <slot name="noTimeIcon"></slot>
          </span>
        </div>
      </div>
      <div
        v-if="
          visibility.times.hours ||
          visibility.times.minutes ||
          visibility.times.seconds
        "
        class="outside"
        @click="clickAwayControls"
      ></div>
      <div
        v-if="
          visibility.controls &&
          !visibility.times.hours &&
          !visibility.times.minutes &&
          !visibility.times.seconds
        "
        class="outside"
        @click="clickAwayParent"
      ></div>
    </div>
  </time-selector-component>
</template>

<script>
import TimeSelectorComponent from './TimeSelectorComponent'
import TimeSelectorControl from './TimeSelectorControl'

export default {
  components: { TimeSelectorComponent, TimeSelectorControl },
  props: {
    hours: {
      type: Array,
    },
    includeSeconds: {
      type: Boolean,
    },
    selectedTime: {
      type: Object,
    },
  },
}
</script>

<style lang="scss" scoped>
.slot-control-wrapper {
  min-width: 40px;
  min-height: 20px;
}
.show-enter-active,
.show-leave-active {
  transition: opacity 0.3s ease-in;
}
.show-enter,
.show-leave-to {
  opacity: 0;
}
.show-enter-to,
.show-leave {
  opacity: 1;
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

button {
  position: relative;
  z-index: 1;
  width: 48px !important;
}
.start-time {
  display: inline-block;
  cursor: default;
}

.container-wrapper {
  position: relative;
  z-index: 2;
}
.grid-container {
  display: grid;
  grid-template-columns: auto auto auto auto;
  position: absolute;
  right: -2em;
  top: -4em;
  background-color: white;
  padding: 8px 12px;
  border: solid 1px #ddd;
  border-radius: 4px;
  z-index: 2;
}
.dashed-border {
  border-bottom: dashed 1px #bbb;
  &.active {
    border-bottom: dashed 1px #333;
  }
}
.slot-control {
  position: relative;
  z-index: 1;
}
.outside {
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0px;
  left: 0px;
  z-index: 0;
}
</style>