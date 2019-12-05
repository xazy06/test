<template>
  <div class="b-actions-bar">
    <div class="b-actions-bar_inner">
      <v-container fluid>
        <v-row
          align="center"
          justify="center"
        >
          <v-btn v-if="remove" @click.stop="removeAction" icon default>
            <v-icon x-large>
              mdi-delete
            </v-icon>
          </v-btn>
          <v-btn v-else @click.stop="action" icon primary>
            <v-icon x-large>
              mdi-check-bold
            </v-icon>
          </v-btn>
        </v-row>
      </v-container>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Actions',
  props: {
    action: {
      type: Function,
      default: () => {}
    },
    remove: Boolean,
    removeAction: {
      type: Function,
      default: () => {}
    }
  },
  methods: {
    cancel () {
      this.$router.push('/')
    }
  }
}
</script>

<style lang="scss" scoped>
  .b-actions-bar {
    transition: all ease-in-out 0.4s;
    position: absolute;
    bottom: -12px;
    left: -12px;
    right: -12px;
    height: 76px;
    border-bottom: 3px solid #e91e63;
    &:hover {
      border-bottom: 7px solid #e91e63;
    }
    &_inner {
      height: 100%;
    }
  }
</style>
