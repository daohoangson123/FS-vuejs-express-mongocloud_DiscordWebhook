<template>
    <div>
        <HelloWorld msg="Discord Form Webhook" />
        <FormWebHook />
    </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import FormWebHook from './components/FormWebHook.vue';

export default {
    name: 'App',
    components: {
        HelloWorld,
        FormWebHook,
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}
</style>
