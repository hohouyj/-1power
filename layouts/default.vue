<template>
  <v-app>

    <v-toolbar app>
      <v-toolbar-title>
        <router-link to="/" tag="span" style="cursor: pointer">
          +1POWER
        </router-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items class="hidden-xs-only">
        <v-btn flat key="Log In" to="/login">
          <v-icon left dark icon="mdi-home"></v-icon>
          LOGIN
        </v-btn>
        <v-btn flat key="SPELLBOOKS" to="/spellbooks">
          <v-icon left dark icon="mdi-book"></v-icon>
          SPELLBOOKS
        </v-btn>
        <v-btn flat key="Log Out" to="/logout">
          <v-icon left dark icon="mdi-logout"></v-icon>
          LOGOUT
        </v-btn>
      </v-toolbar-items>
    </v-toolbar>

    <slot>

    </slot>

  </v-app>
</template>
