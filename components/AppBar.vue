<template>
  <div>
    <v-app-bar
      color="white"
      flat
      class="px-lg-10"
    >
      <v-app-bar-title>
        <nuxt-link to="/" class="home-link">
          App Title
        </nuxt-link>
      </v-app-bar-title>

      <v-spacer></v-spacer>

      <div class="d-none d-sm-flex mr-6">
        <nuxt-link
          v-for="(item, index) in navigation"
          :key="index"
          :to="item.link"
          class="mx-8 font-weight-bold nav"
        >
          {{ item.title }}
        </nuxt-link>
      </div>

      <v-menu offset-y left>
        <!-- Menu button -->
        <template v-slot:activator="{ on, attrs }">
          <v-app-bar-nav-icon
            v-bind="attrs"
            v-on="on"
          ></v-app-bar-nav-icon>
        </template>

        <!-- Popup menu -->
        <v-card width="220">
          <v-list dense nav>
            <v-list-item-group>
              <v-list-item
                v-for="(item, index) in menu"
                :key="index"
                @click="invokeMenuAction(item.action)"
              >
                <v-list-item-icon>
                  <v-icon>{{ item.icon }}</v-icon>
                </v-list-item-icon>

                <v-list-item-content>
                  <v-list-item-title>
                    {{ item.title }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>
      </v-menu>
    </v-app-bar>
  </div>
</template>

<script>
export default {
  data () {
    return {
      navigation: [
        { title: 'Home', link: '/' },
        { title: 'Bookings', link: '/bookings' },
        { title: 'About', link: '' }
      ],
      menu: [
        { title: 'Profile', icon: 'mdi-account-circle', action: '' },
        { title: 'Account Settings', icon: 'mdi-cog', action: '' },
        { title: 'Sign Out', icon: 'mdi-logout', action: 'signOut' }
      ]
    }
  },
  methods: {
    invokeMenuAction (action) {
      this[action]()
    },
    signOut () {
      this.$store.dispatch('user/signOut')
        .then(() => {
          $nuxt.$router.push({ name: 'signin' })
        })
        .catch(err => {
          console.log('Sign out error:', err)
        })
    }
  }
}
</script>

<style scoped>
.home-link {
  color: black;
  text-decoration: none;
  font-weight: bold;
}
.nav {
  color: black;
  text-decoration: none;
}
</style>
