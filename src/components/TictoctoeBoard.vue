<template>
    <div class="game-board-container">
        <div class="row" v-for="(item,i) in 3" :key="`row${i}`">
            <div class="col" v-for="(item,j) in 3" :key="`row${j}`">
                <slot name="boardCol" :index="i*3 +j"></slot>
            </div> 
        </div>
        <button class="tryAgainButton" v-if="tryAgainButton" @click="gameStart">Try Again</button>
    </div>
</template>

<script>
export default{
    name:"TictoctoeBoard",
    props:['mark','tryAgainButton', 'gameStart'],
    data(){
        return{
        }
    },
}
</script>