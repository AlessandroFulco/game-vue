<template>
    <section class="container" id="monster">
        <h2>{{title}} ({{healt}})</h2>
        <div class="healtbar">
            <div class="healtbar__value" :style="barStyles"></div>
        </div>
    </section>
</template>
<script>
import { computed } from 'vue';
export default {
    name : "HeaderComponent",
    props: {
        title:{
            type: String,
            required: true,
            default: "test",
        },
        healt:{
            type: Number,
            required: false,
            default: 0,
        }
    },
    setup(props){

        const barStyles = computed(() => {
            return { width : props.healt + '%'}
        })

        return {
            barStyles
        }
    }
}
</script>

<style>

.healtbar{
    @apply rounded-md border border-gray-500 w-full h-6;
}
.healtbar__value{
    @apply h-full w-full bg-red-400 hover:bg-red-600;
}
</style>