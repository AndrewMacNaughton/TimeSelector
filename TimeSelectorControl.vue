<template functional>
  <div
    v-on:click="listeners.showChild()"
    class="drop-down pointer"
    :class="{ active: props.isVisible }"
  >
    <slot></slot><i class="arrow down"></i>
    <div style="position: relative">
      <div class="control-wrapper">
        <transition name="slide">
          <ul v-if="props.isVisible">
            <li
              @click.stop="listeners.updated(item)"
              v-for="item in props.items"
              :key="item"
              :class="{ selected: item === props.selected }"
            >
              {{ item }}
            </li>
          </ul>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'StartTimeControl',
  props: {
    items: {
      type: Array,
      required: true,
      default: () => [],
    },
    isVisible: {
      type: Boolean,
      required: true,
      default: () => false,
    },
    selected: {
      type: String,
    },
  },
}
</script>

<style lang='scss' scoped>
.slide-enter-active,
.slide-leave-active {
  transition: height .5s ease-out;
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
  ul {
    margin: 0px;
    width: 58px;
    margin-block-start: 0;
    padding-inline-start: 0;
    border: solid 1px #ddd;
    border-top: none;
    position: relative;
    z-index: 2;
    li {
      list-style-type: none;
      padding: 4px;
      z-index: 2;
      &.selected {
        background-color: #eee;
      }
    }
    li:hover {
      background-color: #eee;
    }
  }
}
.arrow {
  border: solid #aaa;
  border-width: 0 2px 2px 0;
  display: inline-block;
  padding: 2px;
  margin-left: 5px;
  margin-bottom: 3px;
  &.down {
    transform: rotate(45deg);
  }
}
.drop-down {
  padding: 8px 12px;
  margin: 4px;
  border-radius: 4px;
  background-color: #f9f9f9;
  border: solid 1px #ddd;
  width: 34px;
  &.active {
    border-bottom-left-radius: 0px;
    border-bottom-right-radius: 0px;
    border-bottom: 0px;
  }
  position: relative;
  z-index: 2;
}
</style>