<template>
    <v-app>
        <v-toolbar app dark>
            <v-toolbar-title class="headline">
                <span class="font-weight-bold green--text">Note-Art JS Demo</span>
            </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn flat href="https://github.com/Seanitzel/note-art"
                   target="_blank">
                <span class="display-1 green--text">Repo</span>
            </v-btn>
            <v-btn flat href="https://note-art-docs.netlify.com/"
                   target="_blank">
                <span class="display-1 green--text">Docs</span>
            </v-btn>
            <v-btn flat href="https://github.com/Seanitzel/Note-Art-Demo"
                   target="_blank">
                <span class="display-1 green--text">Source</span>
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
        name:       'App',
        components: {
            demo,
        },
        data() {
            return {
                //
            }
        },
    }
</script>
