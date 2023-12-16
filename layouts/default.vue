<template>
    <v-app>

        <v-app-bar app>
            <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
            <v-spacer></v-spacer>
            <v-switch :value="$vuetify.theme.dark" label="Dark mode" @change="$vuetify.theme.dark = !$vuetify.theme.dark"
                hide-details></v-switch>

        </v-app-bar>

        <v-navigation-drawer app v-model="drawer">
            <v-list-item href="/">
                <v-list-item-content>
                    <v-list-item-title class="text-h6">
                        Toolkit
                    </v-list-item-title>
                    <v-list-item-subtitle>
                        subtext
                    </v-list-item-subtitle>
                </v-list-item-content>
            </v-list-item>

            <v-divider></v-divider>

            <v-list dense nav>
                <v-list-item v-for="item in items" :key="item.title" link :href="item.link">
                    <v-list-item-icon>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-icon>

                    <v-list-item-content>
                        <v-list-item-title>{{ item.title }}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>

        <v-main>
            <v-container class="px-6 pt-8">
                <Nuxt />
            </v-container>
        </v-main>

        <v-footer app> ver 1.0 </v-footer>
    </v-app>
</template>
<script setup>
import { ref } from 'vue';

const drawer = ref(null)
// const darkMode = ref(null)
</script>

<script>
export default {
    data: () => ({
        drawer: null,
        // darkMode: null,
        items: [
            { title: 'Engineering tolerance', icon: 'mdi-pencil-ruler', link: '/tolerance' },
            { title: 'Clock position', icon: 'mdi-angle-obtuse', link: '/angle' },
            { title: 'About', icon: 'mdi-help-box', link: '/' },
        ],
    }),
    // methods: {
    //     toggleDarkMode: function () {
    //         this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
    //         this.darkMode = this.$vuetify.theme.dark;
    //     },
    // }
}
</script>
<style ></style>
