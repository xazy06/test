<template>
  <div class="b-actions-bar">
    <div class="b-actions-bar_inner">
      <v-container fluid>
        <v-row
          align="center"
          justify="center"
        >
          <v-btn v-if="remove" @click.prevent="removeAction" text default>
            <v-icon x-large>
              mdi-delete
            </v-icon>
          </v-btn>
          <v-btn v-else @click="action" text primary>
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
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    height: 70px;
    &_inner {
      height: 100%;
    }
  }
</style>
