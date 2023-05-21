<template>
    <div>
        <button
            :class="classes"
            @click="handleClick"
            :disabled="disabled"
        >
            {{ title }}
        </button>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue'
export default {
    props: {
        title : {
            type : String,
            required : true,
            default : "Titolo..."
        },
        type : {
            type : String,
            required : false,
            validator(value) {
                return ['success', 'danger'].includes(value);
            },
        },
        disabled : {
            type : Boolean,
            required : false,
        },
    },
    emits: ['evt-click'],

    setup(props, ctx) {
        const classes = ref("");
        const classBase = "btn";

        const handleClick = () =>{
            ctx.emit('evt-click')
        }

        /*hook*/
        onMounted(() =>{
            let classType = "";
            switch (props.type) {
                case "success":
                    classType = "btn-success";
                    break;
                case "danger":
                    classType = "btn-danger";
                    break;
                default:
                    classType = "btn-default"
                    break;
            }
            classes.value = `${classBase} ${classType}`
        })

        return {
            /*variables*/
            classes,
            /*internalTitle,*/
            /*methods*/
            handleClick,
        }
    }
}
</script>

<style>
.btn {
    @apply p-4 rounded-md text-base font-bold text-white;
}
.btn-default {
    @apply bg-blue-600 hover:bg-blue-700;
}
.btn-succes {
    @apply bg-green-600 hover:bg-green-700
}
.btn-danger {
    @apply bg-red-600 hover:bg-red-700
}
:disabled {
    @apply bg-gray-400 hover:bg-gray-400 cursor-not-allowed
}
</style>





