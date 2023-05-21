<template>
    <section class="container">
        <div class="text-3xl">Game Over!</div>
        <div v-if="winner === 'enemy'">You Lost!</div>
        <div v-else-if="winner === 'player'">You Won!</div>
        <div v-else>It's a draw!</div>
        <UiButton title="Nuovo Gioco" @click="handleClick" />
    </section>
</template>

<script>
import UiButton from "@/components/UiButton.vue";

export default {
    name : "GameOverComponent",
    components: {UiButton},
    emits: ["click"],
    props: {
        winner:{
            type: String,
            required: true,
        }
    },
    setup(props, ctx){
        const handleClick = () => {
            ctx.emit("click");
        }
        return {
            /*methods*/
            handleClick,
        }
    }
}
</script>