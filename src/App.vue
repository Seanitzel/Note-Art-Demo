<template>
  <v-app>
    <v-toolbar app dark>
      <v-toolbar-title class="headline">
        <span>Note-Art JS Demo</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
        href="https://seanitzel.github.io/Note-Art/index.html"
        target="_blank"
      >
        <span class="display-1" style="color: brown;">Docs</span>
      </v-btn>
    </v-toolbar>

    <v-content>
      <demo/>
    </v-content>
  </v-app>
</template>

<script>
import demo from './components/demo'

export default {
  name: 'App',
  components: {
    demo
  },
  data () {
    return {
      //
    }
  }
}
</script>
