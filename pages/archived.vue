<template>
  <v-layout
    column
  >
    <v-flex
      xs12
      sm8
      md6
    >
      <v-subheader>
        Your archived templates
      </v-subheader>

      <v-list-item-group
        v-model="selected"
        active-class=""
      >
        <template v-for="(item, index) in items">
          <v-list-item v-if="item.isArchived" :key="item.id">
            <template>
              <v-list-item-content>
                <v-list-item-title v-text="item.title" />
                <v-list-item-subtitle v-text="item.subtitle" />
              </v-list-item-content>
            </template>
          </v-list-item>

          <v-divider
            v-if="index + 1 < items.length && item.isArchived"
            :key="`d${index}`"
          />
        </template>
      </v-list-item-group>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  components: {},
  data: () => ({
    selected: null,
    items: [
      {
        id: 0,
        isArchived: true,
        title: 'title',
        subtitle: 'Description of template'
      },
      {
        id: 1,
        isArchived: true,
        title: 'title',
        subtitle: 'Description of template'
      },
      {
        id: 2,
        isArchived: false,
        title: 'title',
        subtitle: 'Description of template'
      },
      {
        id: 3,
        isArchived: false,
        title: 'title',
        subtitle: 'Description of template'
      },
      {
        id: 4,
        isArchived: false,
        title: 'title',
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
    remove (index) {
      this.items.splice(index, 1)
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
