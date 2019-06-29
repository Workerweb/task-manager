<template>
  <v-app id="inspire" dark>
    <v-navigation-drawer
      app
      temporary
      v-model="sidebar"
      dark
    >
      <v-list>

        <v-list-tile
            v-for="link of links"
            :key="link.title"
            :to="link.url"
            active-class="red--text"
        >

          <v-list-tile-action>
            <v-icon>{{link.icon}}</v-icon>
          </v-list-tile-action>

          <v-list-tile-content>
            <v-list-tile-title v-text="link.title"></v-list-tile-title>
          </v-list-tile-content>

        </v-list-tile>

      </v-list>
    </v-navigation-drawer>

    <v-toolbar dark>

      <v-toolbar-side-icon
        @click="sidebar = !sidebar"
        class="hidden-md-and-up"
      ></v-toolbar-side-icon>

      <v-toolbar-title>Task manager</v-toolbar-title>
      <v-spacer></v-spacer>

      <v-toolbar-items class="hidden-sm-and-down">
        <v-btn
          v-for="link of links"
          :key="link.title"
          :to="link.url"
          flat
        >
          <v-icon left>{{link.icon}}</v-icon>
          {{link.title}}
        </v-btn>
      </v-toolbar-items>

    </v-toolbar>

    <v-content>
    
      <v-container>
        <router-view></router-view> 
      </v-container>

    </v-content>
  </v-app>
</template>

<script>


export default {
  name: 'App',
  data () {
    return {
      sidebar: false,
      links: [
        { title: 'Task list',icon: 'today', url: '/tasks' },
        { title: 'Create task',icon: 'add', url: '/create' },
      ]
    }
  }
}
</script>
