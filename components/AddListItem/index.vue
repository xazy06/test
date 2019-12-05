<template>
  <div @click="action" v-bind:class="{'b-add-bottom': hasElements}" class="b-add">
    <v-icon class="s-em_5">
      mdi-plus
    </v-icon>
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
    height: 80px;
    width: 80px;
    margin: auto;
    &-bottom {
      left: unset;
      top: unset;
      right: 12px;
      bottom: 57px;
      margin: 0;
    }
  }

  a.s-no_underline {
    text-decoration: none;
  }
</style>
