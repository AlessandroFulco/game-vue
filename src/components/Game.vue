<template>
    <div>
        <HeaderComponent :round="round" />
        <GameOverComponent :winner="winner" v-if="winner" @click="newGame"/>

        <div class="my-24">
            <HealtBarComponent title="Nemico" :healt="enemyHealt" />
            <HealtBarComponent title="Io" :healt="playerHealt"/>
        </div>

        <section class="grid grid-cols-2 max-w-xl mx-auto gap-8 my-10">
            <UiButton title="Attacco" @click="attackEnemy"/>
            <UiButton
                title="Attacco Speciale"
                @click="attackEnemySpecial"
                :disabled="attackEnemySpecialDisabled"
            />
            <UiButton
                type="danger"
                title="Medikit"
                @click="medikit"
                :disabled="medikitDisabled"
            />
            <UiButton type="danger" title="Mi Arrendo!" @click="gameover"/>
        </section>

        <BattleLogComponent :log-messages="logMessages" />

    </div>
</template>

<script>
    import { ref, computed, watch } from 'vue';
    import UiButton from "./UiButton.vue";
    import HeaderComponent from "./HeaderComponent.vue";
    import HealtBarComponent from "./HealtBarComponent.vue";
    import GameOverComponent from "./GameOverComponent.vue";
    import BattleLogComponent from "./BattleLogComponent.vue";


    export default{
        components: {BattleLogComponent, GameOverComponent,HealtBarComponent, HeaderComponent, UiButton},
        setup(){
            const round = ref(0);
            const testMsg = ref("messaggio  di prova");
            const playerHealt = ref(100);
            const enemyHealt = ref(100);
            const winner = ref(null);
            const logMessages = ref([]);

            const generateRandomValue = (min, max) => {
                return Math.floor(Math.random() * (max-min) + min);
            }

            /*methods*/
            const attackPlayer = () => {
                const attackValue = generateRandomValue(5, 12);
                if(playerHealt.value - attackValue  <= 0){
                    playerHealt.value = 0;
                }else {
                    playerHealt.value -= attackValue;
                }
                addLogMessage('enemy', 'attack', attackValue);
            }
            const attackPlayerSpecial = () => {
                const attackValue = generateRandomValue(10, 25);
                if(playerHealt.value - attackValue  <= 0){
                    playerHealt.value = 0;
                }else {
                    playerHealt.value -= attackValue;
                }
                addLogMessage('enemy', 'special attack', attackValue);
            }
            const attackEnemy = () => {
                round.value++;
                const attackValue = generateRandomValue(5, 12);
                if(enemyHealt.value - attackValue  <= 0){
                    enemyHealt.value = 0;
                }else {
                    enemyHealt.value -= attackValue;
                }
                addLogMessage('player', 'attack', attackValue);
                attackPlayer();
            }
            const attackEnemySpecial = () => {
                round.value++;
                const attackValue = generateRandomValue(10, 25);
                if(enemyHealt.value - attackValue  <= 0){
                    enemyHealt.value = 0;
                }else {
                    enemyHealt.value -= attackValue;
                }
                addLogMessage('player', 'special attack', attackValue);
                attackPlayerSpecial();
            }
            const medikit = () => {
                round.value++;
                const medikitValue = generateRandomValue(5, 15)
                if(playerHealt.value + medikitValue > 100){
                    playerHealt.value = 100;
                }else {
                    playerHealt.value += medikitValue;
                }
                //addLogMessage('player', 'medikit', medikitValue);
                attackPlayer();
            }
            const gameover = () => {
                winner.value = "enemy";
                playerHealt.value = 0;
            }
            const newGame = () => {
                playerHealt.value = 100;
                enemyHealt.value = 100;
                round.value = 0;
                winner.value = null;
                logMessages.value = [];
            }
            const addLogMessage = (who, what, value) => {
                logMessages.value.unshift({
                    actionBy : who,
                    actionType : what,
                    actionValue : value
                })
            }

            /*computed*/
            const attackEnemySpecialDisabled = computed(() => {
                return round.value % 3 !== 0
            })
            const medikitDisabled = computed(() => {
                return playerHealt.value >= 50 || round.value % 3 !== 0;
            })

            watch(enemyHealt, (enemyHealt, prevEnemyHealt) => {
                if(enemyHealt.value <= 0 && playerHealt.value <= 0){
                    winner.value = "caso di parità";
                }else if(enemyHealt <= 0){
                    winner.value = "player";
                }
            });
            watch(playerHealt, (playerHealt, prevPlayerHealt) => {
                if(playerHealt.value <= 0 && enemyHealt.value <= 0){
                    winner.value = "caso parità";
                }else if(playerHealt <= 0){
                    winner.value = "enemy";
                }
            });

            return {
                round,
                testMsg,
                playerHealt,
                enemyHealt,
                winner,
                logMessages,

                /*methods*/
                attackEnemy,
                attackEnemySpecial,
                medikit,
                gameover,
                newGame,

                /*computed*/
                attackEnemySpecialDisabled,
                medikitDisabled
            }
        }
    }
</script>

<style>
.container{
    @apply text-center p-4 m-4 shadow-xl rounded-md font-bold max-w-xl mx-auto;
}
</style>