<template>
  <v-app>
    <Navbar/>
    <template v-if="error">
      <v-snackbar
        color="error"
        :multi-line="true"
        :timeout="5000"
        @input="closeError"
        value="true"
      >
        {{ error }}

          <v-btn
            dark
            text
            @click.native="closeError"
          >
            Close
          </v-btn>
      </v-snackbar>
    </template>

  </v-app>
</template>

<script>
import Navbar from './components/Navbar.vue'
export default {
  components: {
    Navbar
  },
  methods: {
    closeError () {
      this.$store.dispatch('clearError')
    }
  },
  computed: {
    error () {
      return this.$store.getters.error
    }
  }
}
</script>
