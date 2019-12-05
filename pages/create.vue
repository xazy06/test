<template>
  <v-layout
    column
  >
    <v-flex
      xs12
    >
      <div class="b-ground">
        <div class="b-ground-main">
          <template v-for="(item, index) in blocks">
            <vue-draggable-resizable
              @dragstop="onDragStop"
              @resizestop="onResizeStop"
              @deactivated="onDeactivated"
              @activated="onActivated(item, index)"
              :parent="true"
              :grid="[1,1]"
              :key="index"
            >
              <div v-if="(item.type === 'text')" @click.stop="select(item, index)" contenteditable="true" class="b-ground--text-item">
                {{ item.properties.text }}
              </div>
              <div v-if="(item.type === 'image')" @click.stop="select(item, index)" v-bind:style="{'background-image': `url(${item.properties.src})`}" class="b-ground--img-item" />
            </vue-draggable-resizable>
          </template>
        </div>

        <actions v-if="saveEnabled" :remove="flags.removeEnabled" :removeAction="removeElement" :action="save" />
      </div>
      <types-modal :bridge="blocks" :autoOpen="true" :extra="saveEnabled" />
    </v-flex>
  </v-layout>
</template>

<script>
import VueDraggableResizable from 'vue-draggable-resizable'
import 'vue-draggable-resizable/dist/VueDraggableResizable.css'
import Actions from '~/components/Actions'
import TypesModal from '~/components/TypesModal'
export default {
  components: {
    Actions,
    TypesModal,
    VueDraggableResizable
  },
  data () {
    return {
      selectedIndex: null,
      selected: null,
      flags: {
        removeEnabled: false
      },
      blocks: []
    }
  },
  computed: {
    saveEnabled () {
      return this.blocks.length > 0
    }
  },
  watch: {
    selectedIndex (val) {
      this.flags.removeEnabled = val !== null
    }
  },
  methods: {
    save () {
      this.$router.push('/')
    },
    select (item, index) {
      this.selectedIndex = index
      this.selected = item
    },
    unselect () {
      this.selectedIndex = null
      this.selected = null
    },
    onResizeStop (x, y, width, height) {
      if (this.selected !== null) {
        this.selected.properties.width = width
        this.selected.properties.height = height
      }
    },
    onDragStop (x, y) {
      if (this.selected !== null) {
        this.selected.properties.left = x
        this.selected.properties.top = y
      }
    },
    onActivated (item, index) {
      this.select(item, index)
    },
    onDeactivated () {
      setTimeout(() => { this.unselect() }, 0)
    },
    removeElement () {
      this.blocks.splice(this.selectedIndex, 1)
      this.unselect()
    }
  }
}
</script>

<style lang="scss" scoped>
  .b-ground {
    width: 100%;
    height: calc(100vh - 118px);
    position: relative;
    &-main {
      height: calc(100vh - 118px - 57px);
      padding: 0 20px;
    }
    &--img-item {
      height: 100%;
      background-position: center center;
      background-size: cover;
      background-repeat: no-repeat;
      overflow: hidden;
    }
    &--text-item {
      height: 100%;
      overflow: hidden;
    }
  }
</style>
