<template>
    <v-app>
        <v-content>
            <router-view />
        </v-content>

        <Navbar v-if="!$route.meta.hideNavigation" />
    </v-app>
</template>

<style lang="scss">
body {
    font-family: $body-font;
    font-size: 18px;
    color: $text-color;

    h1 {
        color: $blue;
        font-size: 2.8rem;
        font-family: $heading-font;
        margin: 0;
        padding-top: 20px;
        text-align: center;
        line-height: 1;
        font-weight: 400;
    }

    h2 {
        font-size: 2rem;
        font-family: $heading-font;
        color: $blue;
        font-weight: 400;
    }

    h3 {
        font-size: 1rem;
        font-family: $body-font;
        color: $text-color;
    }
}
</style>

<script>
import Navbar from './components/Navbar.vue';
export default {
    name: 'App',

    components: {
        Navbar
    }
};
</script>
