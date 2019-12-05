<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" icon="hamburger" />
      <v-toolbar-title v-text="title" />
      <v-spacer />
    </v-app-bar>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
    <v-footer
      :fixed="fixed"
      v-touch:swipe.top="dialogInit"
      app
    >
      <span>&copy; nuxt</span>
    </v-footer>
    <notifications group="foo" />
  </v-app>
</template>

<script>
export default {
  transition: 'edit',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Templates',
          to: '/'
        },
        {
          icon: 'mdi-archive',
          title: 'Archive',
          to: '/archived'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'SIMPLE CONTENT EDITOR'
    }
  },
  methods: {
    dialogInit () {
    }
  }
}
</script>
