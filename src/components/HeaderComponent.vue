<template>
    <div class="header">
        <h1>Game | Round: {{round}}</h1>
    </div>
</template>
<script>
export default {
    name : "HeaderComponent",
    props: {
        round:{
            type: Number,
            required: true,
            default: "test",
        }
    },
    setup(props){

        return {

        }
    }
}
</script>

<style>
    .header{
        @apply bg-green-400 text-center p-4 text-xl font-bold mb-20;
    }
</style>