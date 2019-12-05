<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" fullscreen hide-overlay transition="dialog-bottom-transition">
      <template v-slot:activator="{ on }">
        <div v-on="on">
          <add-list-item v-on="on" :has-elements="extra" />
        </div>
      </template>
      <v-card>
        <v-toolbar dark color="primary">
          <v-toolbar-items>
            <v-btn @click="drop" dark text>
              <v-icon>mdi-arrow-left</v-icon>
            </v-btn>
          </v-toolbar-items>
          <v-toolbar-title>
            Select one
          </v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn @click="cancel" dark text>
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-container fluid>
          <v-fade-transition mode="out-in">
            <v-row
              key="0"
              v-if="showThumbs"
            >
              <v-col v-for="(n, index) in 6" :key="index" cols="4">
                <v-card @click="selectImg('https://picsum.photos/350/165?random')">
                  <v-img
                    src="https://picsum.photos/350/165?random"
                    height="100"
                    class="grey darken-4"
                  />
                </v-card>
              </v-col>
            </v-row>
          </v-fade-transition>
          <v-fade-transition mode="out-in">
            <v-row key="1" v-if="!showThumbs">
              <v-col cols="12">
                <v-row
                  align="center"
                  justify="space-around"
                >
                  <v-card
                    v-for="(item, index) in types"
                    :key="index"
                    @click.stop="select(item)"
                    class="ma-3 pa-6"
                    default
                    elevation="0"
                  >
                    <v-icon x-large class="s-em_14">
                      {{ item.icon }}
                    </v-icon>
                    <div class="text-center">
                      {{ item.name }}
                    </div>
                  </v-card>
                </v-row>
              </v-col>
            </v-row>
          </v-fade-transition>
        </v-container>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
import AddListItem from '~/components/AddListItem'
export default {
  name: 'TypesModal',
  components: {
    AddListItem
  },
  props: {
    extra: {
      type: Boolean
    },
    bridge: {
      type: Array,
      default: () => {
        return []
      }
    },
    autoOpen: Boolean
  },
  data () {
    return {
      dialog: false,
      showThumbs: false,
      types: [{
        id: 0,
        type: 'text',
        name: 'text',
        icon: 'mdi-pen-plus',
        properties: {
          left: 0,
          top: 0,
          width: 150,
          height: 150,
          text: ''
        }
      },
      {
        id: 1,
        type: 'image',
        name: 'image',
        icon: 'mdi-image-plus',
        properties: {
          left: 0,
          top: 0,
          width: 150,
          height: 150,
          src: 'https://picsum.photos/350/165?random'
        }
      }],
      selected: null
    }
  },
  mounted () {
    if (this.autoOpen === false) {
      return
    }

    this.dialog = true
  },
  methods: {
    cancel () {
      this.dialog = false
      this.showThumbs = false
    },
    drop () {
      this.cancel()
      this.$router.push('/')
    },
    resolveSelection (type) {
      this.bridge.push(type)
      this.dialog = false
      this.showThumbs = false
      this.selected = null
    },
    select (type) {
      this.selected = type

      if (this.selected.type === 'image') {
        this.showThumbs = true
        return this.selected
      }

      return this.resolveSelection(this.selected)
    },
    selectImg (src) {
      this.selected.properties.src = src
      this.resolveSelection(this.selected)
    }
  }
}
</script>

<style scoped>

</style>
