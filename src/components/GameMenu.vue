<template>
  <div class="gameMenu">
    <div>
      <div class="choiceDifficulty button" @click="choiceDifficultyButton">
        <span class="tips">选择难度</span>
      </div>
      <div class="newGame button" @click="newGameButton">
        <span class="tips">新游戏</span>
      </div>
      <div class="undo button" @click="revokeButton" :style="{
          backgroundImage:`url('${(value && value.allowRevoke)?img.undo:img.undoDisable}')`,
          cursor: value && value.allowRevoke ? 'default' : 'not-allowed'
        }">
        <span class="tips">撤销</span>
      </div>

      <div class="choiceDifficultyBox" v-if="showChoiceDifficulty"  @click="choiceDifficultyClose">
        <div class="choiceList" @click.stop="()=>{}">
          <div class="close" @click="choiceDifficultyClose">X</div>
          <ul>
            <li @click="choiceDifficulty(0)">初级（单色）</li>
            <li @click="choiceDifficulty(1)">中级（双色）</li>
            <li @click="choiceDifficulty(2)">高级（四色）</li>
          </ul>
        </div>
      </div>
    </div>
    <div>
      <div class="scoreBoard">

      </div>
    </div>
  </div>
</template>

<script>

import Scoreboard from "@/components/Scoreboard";

export default {
  name: "GameMenu",
  data(){
    return{
      showChoiceDifficulty: true,
      img: {
        undo: require("@/assets/button/undo.png"),
        undoDisable: require("@/assets/button/undo_disable.png")
      }
    }
  },
  props:{
    value:Object
  },
  model:{
    prop: "value",
    // event:"change"
  },
  watch:{
    value: {
      deep: true,
      handler(){
        console.log("value change")
      }
    }
  },
  components:{
    Scoreboard
  },
  methods:{
    choiceDifficultyButton(){
      this.showChoiceDifficulty = true;
    },
    newGameButton(){
      this.$emit("menu",{ type: 1 })
    },
    revokeButton(){
      this.$emit("menu",{ type: 2 })
    },
    choiceDifficultyClose(){
      this.showChoiceDifficulty = false;
    },
    choiceDifficulty(grade){
      this.showChoiceDifficulty = false;
      this.$emit("menu",{ type:0, grade })
    }
  }
}
</script>

<style scoped>
.gameMenu{
  width: 100vw;
  height: 70px;
  position: fixed;
  padding: 0 20px;
  box-sizing: content-box;
  left: 0;
  bottom: 0;
  z-index: 999;
}
.gameMenu>div>.button{
  display: inline-block;
  width: 60px;
  height: 60px;
  background-repeat: no-repeat;
  background-size: 80%;
  background-position: center center;
  border-radius: 10px;
  margin: 0 5px;
  position: relative;
}
.gameMenu>div>div>span{
  position: absolute;
  color: white;
  font-size: 12px;
  background-color: rgba(0,0,0,0.8);
  padding: 5px;
  top: -30px;
  opacity: 0;
  border-radius: 5px;
}
.gameMenu>div>.button:hover{
  background-color: rgba(0,0,0,0.5);
}
.gameMenu>div>div:hover>span{
  opacity: 1;
}
.choiceDifficulty{
  background-image: url("@/assets/button/choice_difficulty.png");
}
.newGame{
  background-image: url("@/assets/button/new_game.png");
}
.undo{
  background-image: url("@/assets/button/undo.png");
}


.choiceDifficultyBox{
  position: fixed;
  left: 0;
  top: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0,0,0,0.7);
}
.choiceDifficultyBox ul{
  margin: 0;
  padding: 0;
  list-style: none;
}
.choiceDifficultyBox .choiceList{
  width: 120px;
  position: fixed;
  left: calc(50% - 60px);
  top: calc(50% - 100px);
  border-radius: 10px;
  padding: 30px 20px;
  background-color: white;
}
.choiceDifficultyBox>ul{
  width: 100%;
  height: 100%;
}
.choiceDifficultyBox .close{
  width: 10px;
  height: 10px;
  position: absolute;
  right: 10px;
  top: 5px;
  color: black;
  cursor: pointer;
}
.choiceDifficultyBox li{
  width: 100%;
  line-height: 30px;
  text-align: center;
  border-bottom: 1px solid rgba(0,0,0,0.5);
  cursor: pointer;
}
.choiceDifficultyBox li:hover{
  color: white;
  background-color: rgba(0,0,0,0.7);
}

</style>