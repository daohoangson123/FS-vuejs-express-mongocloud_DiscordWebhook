<template>
    <div class="hello">
        <h1 class="text-center text-indigo-700 text-4xl font-bold p-3">
            {{ msg }}
        </h1>
    </div>
</template>

<script>
export default {
    name: 'HelloWorld',
    props: {
        msg: String,
    },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
