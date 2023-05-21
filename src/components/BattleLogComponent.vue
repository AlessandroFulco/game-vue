<template>
    <section id="log" class="container">
        <h2>Battle Log || {{ logMessages.length }} azioni</h2>
        <ul class="list-disc text-left mx-8">
            <li v-for="logMessage in logMessages" class="py-2">
                    <span :class="{
                                'bg-blue-500 p-2 rounded-xl': logMessage.actionBy === 'player',
                                'bg-yellow-500 p-2 rounded-xl': logMessage.actionBy === 'enemy'
                            }">
                        {{ logMessage.actionBy === 'player' ? 'Player' : 'Enemy' }}
                    </span>
                <span v-if="logMessage.actionType === 'medikit'">
                    usa il medikit con
                    <span class="bg-green-500 p-2 rounded-xl">{{ logMessage.actionValue }} punti</span>
                </span>
                <span v-else>
                    attacca con
                    <span class="bg-red-500 p-2 rounded-xl">{{ logMessage.actionValue }} punti</span>
                </span>
            </li>
        </ul>
    </section>
</template>

<script>
export default {
    name : "BattleLogComponent",
    props: {
        logMessages:{
            type: Array,
            required: true,
        }
    },
    setup(props, ctx){

        return {

        }
    }
}
</script>

