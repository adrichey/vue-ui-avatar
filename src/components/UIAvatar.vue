<template>
  <div
    class="ui-avatar"
    :style="{
      'background-color': color,
      'background-image': image ? `url('${image}')` : null,
    }"
    @mouseover="showTooltip = true"
    @mouseout="showTooltip = false"
  >
    <div
      ref="status"
      v-if="status"
      class="ui-avatar__status-circle"
      :class="status"
    />

    <transition name="slide-fade">
      <span
        ref="tooltip"
        v-show="showTooltip"
        class="ui-avatar__tooltip"
      >{{ label }}</span>
    </transition>

    <span ref="initials" v-if="!image">{{ initials }}</span>
  </div>
</template>

<script>
export default {
  name: 'ui-avatar',
  props: {
    label: {
      type: String,
      required: true,
    },
    image: {
      type: String,
      default: '',
    },
    status: {
      type: String,
      default: null,
      validator (value) {
        return ['online', 'available', 'dnd'].indexOf(value) !== -1;
      }
    },
    color: {
      type: String,
      default: '#3a174c',
    },
  },
  data() {
    return {
      showTooltip: false,
    }
  },
  computed: {
    initials() {
      const labelParts = this.label.split(' ');

      if (labelParts.length > 1) {
        const firstInitial = labelParts[0].substring(0, 1).toUpperCase();
        const lastInitial = labelParts[1].substring(0, 1).toUpperCase();

        return `${firstInitial}${lastInitial}`;
      }

      return this.label.substring(0, 2).toUpperCase();
    },
    statusStyles() {
      if (!this.status) {
        return {}
      }

      return {
        'background-color': STATUSES[this.status],
      };
    },
  },
}
</script>

<style lang="scss" scoped>
$tooltip-height: 32px;

.ui-avatar {
  position: relative;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  font-size: 1em;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  font-weight: bold;
  line-height: 40px;
  color: #fff;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-size: cover;
  z-index: 9998;
}

.ui-avatar__tooltip {
  background-color: #000;
  border-radius: ($tooltip-height / 8);
  height: $tooltip-height;
  line-height: $tooltip-height;
  margin-top: ($tooltip-height / 8);
  padding: 0 ($tooltip-height / 4);
  position: absolute;
  top: 0;
  left: 0;
  margin-left: 52px;
  width: max-content;
  z-index: 9999;

  // 16px tall arrow
  &:before {
    border-top: ($tooltip-height / 4) solid transparent;
    border-bottom: ($tooltip-height / 4) solid transparent;
    border-right: ($tooltip-height / 4) solid #000;
    content: '';
    height: 0;
    position: absolute;
    left: -($tooltip-height / 4);
    top: ($tooltip-height / 4);
    width: 0;
  }
}

.ui-avatar__status-circle {
  display: block;
  width: 8px;
  height: 8px;
  position: absolute;
  right: 0;
  margin-right: -2px;
  z-index: 9999;
  border: 2px solid #fff;
  border-radius: 50%;

  &.online {
    background-color: #00cc00;
  }

  &.available {
    background-color: #1e90ff;
  }

  &.dnd {
    background-color: #ff3300;
  }
}

.slide-fade-enter-active, .slide-fade-leave-active {
  transition: all .3s ease;
}

.slide-fade-enter, .slide-fade-leave-to {
  transform: translateX(-5px);
  opacity: 0;
}
</style>
