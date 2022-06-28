<template>
  <v-app dark>
    <v-navigation-drawer
    v-if="false"
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
    color="#404040"
     v-if="$route.name!='index'"
      :clipped-left="clipped"
      fixed
      app
    >
    <div class="white--text">
      <v-row>
        <v-col align-self="center">
          EARIST
        </v-col>
        <v-col>
            <v-img height="50" width="50" src="./CAFA LOGO.png"></v-img>
        </v-col>
        <v-col>
            <v-img height="50" width="50" src="./CAS LOGO.png"></v-img>
        </v-col>
        <v-col>
            <v-img height="50" width="50" src="./CBPA LOGO.png"></v-img>
        </v-col>
        <v-col>
            <v-img height="50" width="50" src="./CED LOGO.png"></v-img>
        </v-col>
        <v-col>
            <v-img height="50" width="50" src="./CEN LOGO.png"></v-img>
        </v-col>
        <v-col>
            <v-img height="50" width="50" src="./CEN-SG LOGO.png"></v-img>
        </v-col>
        <v-col>
            <v-img height="50" width="50" src="./CHTM LOGO.png"></v-img>
        </v-col>
        <v-col>
            <v-img height="50" width="50" src="./CIT LOGO.png"></v-img>
        </v-col>
      </v-row>
    </div>
      <v-spacer></v-spacer>
    <div @click="goHome" class="white--text">
      <v-icon color="white">mdi-home</v-icon>Home
    </div>
    <div @click="logoutPage" class="white--text">
      <v-icon class="pl-5" dark>mdi-logout</v-icon>Logout
    </div>
    </v-app-bar>
    <v-main>
      <v-container class="pa-0" fluid>
        <Nuxt />
      </v-container>
    </v-main>
    <v-navigation-drawer
    v-if="false"
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
    >
      <v-list>
        <v-list-item @click.native="right = !right">
          <v-list-item-action>
            <v-icon light>
              mdi-repeat
            </v-icon>
          </v-list-item-action>
          <v-list-item-title>Switch drawer (click me)</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  methods:{
    logoutPage(){
      window.location.href="/"
    },
    goHome(){
      window.location.href="/directory"
    }
  },
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js'
    }
  }
}
</script>
