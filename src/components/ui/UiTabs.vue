<template>
  <div
    aria-role="tablist"
    class="ui-tabs border-b flex items-center relative"
    @mouseleave="showHoverIndicator = false"
  >
    <div
      v-show="showHoverIndicator"
      ref="hoverIndicator"
      class="
        ui-tabs__indicator
        z-0
        top-[5px]
        absolute
        left-0
        rounded-lg
        bg-box-transparent
      "
    ></div>
    <slot></slot>
  </div>
</template>
<script>
import { provide, toRefs, ref } from 'vue';

export default {
  props: {
    modelValue: {
      type: [String, Number],
      default: '',
    },
    fill: Boolean,
  },
  emits: ['update:modelValue'],
  setup(props, { emit }) {
    const hoverIndicator = ref(null);
    const showHoverIndicator = ref(false);

    function updateActive(id) {
      emit('update:modelValue', id);
    }
    function hoverHandler({ target }) {
      showHoverIndicator.value = true;
      hoverIndicator.value.style.width = `${
        target.getBoundingClientRect().width
      }px`;
      hoverIndicator.value.style.transform = `translateX(${target.offsetLeft}px)`;
    }

    provide('ui-tabs', {
      updateActive,
      hoverHandler,
      ...toRefs(props),
    });

    return {
      hoverIndicator,
      showHoverIndicator,
    };
  },
};
</script>
<style>
.ui-tabs__indicator {
  min-height: 38px;
  min-width: 50px;
  transition-duration: 200ms;
  transition-property: transform, width;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
}
</style>