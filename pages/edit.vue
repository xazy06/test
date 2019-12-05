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
              :w="item.properties.width"
              :h="item.properties.height"
              :y="item.properties.top"
              :x="item.properties.left"
              @dragstop="onDragStop"
              @resizestop="onResizeStop"
              @deactivated="onDeactivated"
              @activated="onActivated(item, index)"
              :parent="true"
              :grid="[1,1]"
              :key="index"
            >
              <div v-if="(item.type === 'text')" @click="select(item, index)" contenteditable="true" class="b-ground--text-item">
                {{ item.properties.text }}
              </div>
              <div
                v-if="(item.type === 'image')"
                @click="select(item, index)"
                v-bind:style="{'background-image': `url(${item.properties.src})`}"
                class="b-ground--img-item"></div>
            </vue-draggable-resizable>
          </template>
        </div>

        <actions v-touch:swipe.top="showTypes" v-if="saveEnabled" :remove="flags.removeEnabled" :removeAction="removeElement" :action="save" />
      </div>
      <types-modal :toggleHidden="true" ref="typesModal" :bridge="blocks" :autoOpen="false" :extra="saveEnabled" />
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
      blocks: [
        {
          id: 0,
          type: 'image',
          name: 'image',
          properties: {
            left: 0,
            top: 0,
            width: 250,
            height: 150,
            src: 'https://picsum.photos/350/165?random'
          }
        },
        {
          id: 1,
          type: 'text',
          name: 'text',
          properties: {
            left: 0,
            top: 200,
            width: 150,
            height: 50,
            text: 'Solves.pro'
          }
        }
      ]
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
      if (this.selectedIndex === null) {
        throw new Error('unexpected selectedIndex value')
      }
      this.blocks.splice(this.selectedIndex, 1)
      this.unselect()
    },
    showTypes () {
      this.$refs.typesModal.dialog = true
    }
  }
}
</script>

<style lang="scss" scoped>
  .b-ground {
    width: 100%;
    height: calc(100vh - 80px);
    position: relative;
    &-main {
      height: calc(100vh - 118px - 57px)
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
