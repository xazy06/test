<template>
  <v-layout
    column
  >
    <v-flex
      xs12
      sm8
      md6
      s-pb_52
    >
      <v-subheader v-if="hasTemplates">
        Your templates
      </v-subheader>
      <v-subheader v-else>
        Add your first template
      </v-subheader>

      <v-list-item-group
        v-model="selected"
        active-class=""
      >
        <transition-group name="slide-fade" appear>
          <template v-for="(item, index) in items">
            <v-list-item v-touch:swipe.right="remove(index)" v-touch:swipe.left="archive(item)" @click="go('edit')" :key="item.id">
              <template>
                <v-list-item-avatar>
                  <v-icon v-if="item.isArchived === false" :class="['blue white--text']">mdi-image-edit-outline</v-icon>
                  <v-icon v-else :class="['grey white--text']">mdi-archive</v-icon>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title v-text="item.title" />
                  <v-list-item-subtitle v-text="item.subtitle" />
                </v-list-item-content>
              </template>
            </v-list-item>

            <v-divider
              v-if="index + 1 < items.length"
              :key="`d${index}`"
            />
          </template>
        </transition-group>
      </v-list-item-group>
      <add-list-item :action="create" :has-elements="hasTemplates" />
    </v-flex>
  </v-layout>
</template>

<script>
import AddListItem from '~/components/AddListItem'
export default {
  components: {
    AddListItem
  },
  data: () => ({
    selected: null,
    items: [
      {
        id: 0,
        isArchived: true,
        title: 'Title',
        subtitle: 'Description of template'
      },
      {
        id: 1,
        isArchived: false,
        title: 'Title',
        subtitle: 'Description of template'
      },
      {
        id: 2,
        isArchived: false,
        title: 'Title',
        subtitle: 'Description of template'
      },
      {
        id: 3,
        isArchived: false,
        title: 'Title',
        subtitle: 'Description of template'
      },
      {
        id: 4,
        isArchived: false,
        title: 'Title',
        subtitle: 'Description of template'
      }
    ]
  }),
  computed: {
    hasTemplates () {
      return this.items.length > 0
    }
  },
  methods: {
    go (routeName) {
      this.$router.push(routeName)
    },
    create () {
      return this.go('create')
    },
    archive (item) {
      return function (d, e) {
        item.isArchived = !item.isArchived
        this.notify('Action', `Element was ${['un', ''][+item.isArchived]}archived`)
      }.bind(this)
    },
    remove (index) {
      return function (d, e) {
        this.items.splice(index, 1)
        this.notify('Action', 'Removed')
      }.bind(this)
    },
    notify (ttl, txt) {
      this.$notify({
        group: 'foo',
        title: ttl,
        text: txt
      })
    }
  }
}
</script>

<style lang="scss" scoped>
  .s-pb_52 {
    padding-bottom: 52px;
  }
</style>
