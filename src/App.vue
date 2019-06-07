<template>
  <v-app>
    <v-content>
      <Contact/>
    </v-content>
  </v-app>
</template>

<script>
import Contact from './components/contact'

export default {
  name: 'App',
  components: {
    Contact
  },
  data () {
    return {
      //
    }
  }
}
</script>
