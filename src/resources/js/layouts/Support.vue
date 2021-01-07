<template>
  <v-app>
    <v-system-bar app>
      <v-spacer />
    </v-system-bar>

    <v-app-bar clipped-left app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />

      <v-toolbar-title>Support Ticket System</v-toolbar-title>

      <v-spacer />

      <v-btn
        text
        aria-label="Theme Button"
        @click="$vuetify.theme.dark = !$vuetify.theme.dark"
      >
        <v-icon> mdi-theme-light-dark </v-icon>
      </v-btn>

      <v-progress-linear
        :active="loading"
        :indeterminate="loading"
        bottom
        absolute
        color="grey"
      />
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" clipped app>
      <v-sheet v-if="isLogged" class="pa-4">
        <div class="d-flex align-center mb-4">
          <v-avatar class="grey" size="64">
            <span>J.V</span>
          </v-avatar>

          <div class="text-center font-weight-medium ml-10">
            {{ user.roles[0].name }}
          </div>
        </div>

        <div v-if="user" class="font-weight-light">
          {{ user.email }}
        </div>
      </v-sheet>

      <v-divider v-if="isLogged" />

      <v-list class="pa-0">
        <v-list-item v-if="isLogged" :to="{ name: user.roles[0].name }" link>
          <v-list-item-icon>
            <v-icon>mdi-home</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-list-item v-if="isLogged" @click="logoutUser">
          <v-list-item-icon>
            <v-icon>mdi-logout</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>Logout</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container
        class="px-6"
        :class="{ 'py-2': drawer, 'py-4': !drawer }"
        fluid
      >
        <slot />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { mapState, mapGetters, mapActions } from 'vuex';

export default {
  name: 'Support',
  data() {
    return {
      drawer: true,
      loading: true
    };
  },
  computed: {
    ...mapGetters('auth', {
      isLogged: 'isLogged'
    }),
    ...mapState('auth', {
      user: 'user'
    })
  },
  mounted() {
    this.loading = false;
  },
  methods: {
    ...mapActions('auth', {
      logout: 'logout'
    }),
    async logoutUser() {
      await this.logout();

      await this.$router.push({ name: 'login' });
    }
  }
};
</script>

<style scoped></style>