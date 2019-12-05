<template>
  <div @click="action" v-bind:class="{'b-add-bottom': hasElements}" class="b-add">
    <transition name="rotate-fade" appear>
      <v-icon class="s-em_4">
        mdi-plus
      </v-icon>
    </transition>
  </div>
</template>
<script>
export default {
  name: 'AddListItem',
  props: {
    hasElements: {
      type: Boolean
    },
    action: {
      type: Function,
      default: () => {}
    }
  }
}
</script>

<style lang="scss" scoped>
  .b-add {
    position: fixed;
    right: 0;
    left: 0;
    bottom: 0;
    top: 0;
    margin: auto;
    height: 64px;
    width: 64px;
    &-bottom {
      left: unset;
      top: unset;
      right: 12px;
      bottom: 21px;
      margin: 0;
    }
  }

  a.s-no_underline {
    text-decoration: none;
  }
</style>
