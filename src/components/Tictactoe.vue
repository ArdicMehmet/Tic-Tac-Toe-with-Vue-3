<template>
    <div class="tictoctoe">
        <h1>Tictoctoe</h1>
        <h3 class="user-winner" >{{  winner }}</h3>
        <h3 class="user-turn">{{ !winner ? `Sıra ${currentUser%2 === 1 ? '1' : '2'} numaralı oyuncuda` :'' }}</h3>
        <TictoctoeBoard :tryAgainButton=tryAgainButton :gameStart=gameStart>
        <template v-slot:boardCol="{index : index}">
            <div class="chooseBox" :data-id="index" :class="[gameBoard.boardElements[index].pointerNone ? 'notClick' : '' ,gameBoard.boardElements[index].bg]" @click="mark">{{ gameBoard.boardElements[index].content }}</div>
        </template>
        </TictoctoeBoard>
    </div>
</template>

<script>
import TictoctoeBoard from './TictoctoeBoard.vue';
    export default {
        name:"Tictoctoe Component",
        created(){
            this.createBoard();
        },
        data(){
            return{
                currentUser : 1,
                winner : "",
                winCondutionArray :[[0,1,2],[3,4,5],[6,7,8],[0,3,6],[1,4,7],[2,5,8],[0,4,8],[2,4,6]],
                gameBoard:{
                    boardElements : [],
                    board_move : [],
                },
                match : true,
                tryAgainButton : false,
            }
        },
        components:{
            TictoctoeBoard,
        },
        methods :{
            mark(event){
                let selectionId = event.currentTarget.getAttribute('data-id');
                let targetBlock = this.gameBoard.boardElements[selectionId];
                targetBlock.pointerNone = true;
                if(this.currentUser%2 === 1){
                    targetBlock.content = "X";
                    this.addMove(selectionId,'X');
                }
                else{
                    targetBlock.content = "O";
                    this.addMove(selectionId,'O');
                }
                this.currentUser+=1;
            },
            gameStart(){
                this.gameBoard.boardElements = [];
                this.createBoard();
                this.gameBoard.board_move =[];
                this.winner = "";
                this.currentUser = 1;
                this.tryAgainButton = false;
            },
            createBoard(){
                for(let i =0; i<9;i++){
                    this.gameBoard.boardElements.push({ id :i, content :"",pointerNone: false, bg:''}); 
                }
            },
            addMove(id, type){
                this.gameBoard.board_move[id] = {boardId : id, status : type};
                if(this.winCondution(id, type)){
                    this.gameOver(`Kazanan ${type === 'X' ? '1' : '2'} numaralı oyuncu`);
                }
            },
            winCondution(id,type){
                const interestedArray = this.winCondutionArray.filter(condution => condution.includes(parseInt(id)));
                this.match = true;
                for(let i = 0; i< interestedArray.length; i++){
                    this.match = true;
                    for(let j =0 ;j< interestedArray[i].length; j++){
                        if(this.gameBoard.board_move[interestedArray[i][j]]){
                            if(this.gameBoard.board_move[interestedArray[i][j]].status != type){
                                this.match = false;
                            }
                        }
                        else{
                            this.match = false;
                        }
                    }
                    if(this.match){
                        let matchedArray = interestedArray[i];
                        for(let i =0; i< matchedArray.length; i++){
                            this.gameBoard.boardElements[matchedArray[i]].bg = 'bg-green';
                        }
                        return true;
                    }   
                }
                return false;
            },
            gameOver(message){
                this.winner = message;
                for(let index= 0; index<this.gameBoard.boardElements.length; index++){
                    this.gameBoard.boardElements[index].pointerNone = true;
                }
                this.tryAgainButton = true;
            }
        },
        watch:{
            currentUser(newTurn){
                if(newTurn>=10){
                    this.gameOver('Maç Berabere Bitti');
                }
            }
        }
    }
</script>
<style>
.tictoctoe{
    text-align: center;
    transform: translateY(-20%);
}
 h1{
    margin-bottom: 20px;
 }
 .tryAgainButton{
    padding: 7px 12px;
    font-size: 20px;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    background-color: rgb(57, 236, 48);
    opacity: .7;
    outline: none;
    border: none;
    margin: 20px 0;
 }
 .tryAgainButton:hover{
    opacity: 1;
    cursor: pointer;
 }
 .bg-green{
    background-color: rgb(57, 236, 48);
 }
 .user-turn, .user-winner{
    margin-bottom: 20px;
 }
 .game-board-container{
    width: 300px;
    height: 300px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
 }
 .row{
    min-width: 300px;
    width: 100%;
    min-height: 100px;
    height: 100px;
    background-color:burlywood;
    flex-shrink: 1;
    display: flex;
    border-bottom: 3px solid white;
 }

 .row:last-child{
    border-bottom: none;
 }
 .col{
    min-width: 100px;
    width: 100px;
    min-height: 100px;
    height: 100%;
    flex-shrink: 1;
    border-left: 3px solid white;
 }
 .col:first-child{
    border-left: none;
 }
 .chooseBox{
    width:100% ;
    height: 100%;
    font-size: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
 }
 .chooseBox:hover{
    cursor: pointer;
 }
 .notClick {
    pointer-events: none;
 }
 </style>