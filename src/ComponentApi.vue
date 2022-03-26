<template>
  <div class="container">
    <h1 v-show="level > 4">You Win all the levels</h1>
    <h1 v-show="level <= 4">Level {{ level }}</h1>
    <div class="wrapper" id="wrapper" v-show="level <= 4">
          <div class="bottle" v-for="(bt, i) in gameArray" :key="i"
          @click="BottleHandler" :data-number="i">
            <transition-group name="color">
              <Confet v-if="bottleFull(bt)" />
              <div v-for="(b, j) in bt" :key="j" 
              :class="`color ${colorPick(b)}`" 
              v-show="b !== 0"></div>
            </transition-group>
          </div>
      </div>
  </div>
</template>

<script>
import levels from '@/levels'
import Confet from '@/Confet.vue'
import { ref, onBeforeMount } from 'vue'

export default {
  name: "Setup",
  components: {
    Confet
  },
  setup(){
    let gameArray = ref([]);
    let hbh = false;
    let hel = null;
    let btlw = null;
    let level = 1;
    const levelUp = () => {
      gameArray.value = levels[level - 1].map
    }
    const colorPick = (n) => {
      switch (n) {
        case 1:
        return "red";
        case 2:
        return "blue";
        case 3:
        return "green";
        case 4:
        return "yellow";
        case 5:
        return "orange";
        case 6:
        return "purple";
        case 7:
        return "brown";
        case 8:
          return "pink";
        case 9:
          return "green";
        case 10:
          return "deeppink";
      }
    }
    const bottleFull = (arr) => {
      let x = arr[0];
      let s = 0;
      for(let i=0;i<4;i++){
        if(x == arr[i] && x !== 0){
          s++;
        }
      }
      if(s == 4){
        return true;
      }
      return false;
    }
    const BottleHandler = (e) => {
      if (!hbh) {
        hel = e.target;
        hel.classList.remove("isSwap");
        btlw = parseInt(hel.getAttribute("data-number"));
        e.target.classList.add("isHand");
        hbh = true;
      } else {
        const btls = document.querySelectorAll(".bottle");
        if(hel != e.target){
        cotrolGame(
            btlw,
            parseInt(e.target.getAttribute("data-number"))
            );
        }
        btls.forEach((b) => b.classList.remove("isHand"));
        hbh = false;
        hel = null
      }
    }
    const cotrolGame = (a, b) => {
      let x1 = enW(gameArray.value[a]);
      let x2 = enW(gameArray.value[b]);
      let stp = null;
      if (
          (gameArray.value[a][x1] !== undefined && 
          gameArray.value[b][x2] == gameArray.value[a][x1] && x2 !== 0) ||
          (gameArray.value[a][x1] !== undefined && x2 === 4)
      ) {
          stp = gameArray.value[a][x1];
          while(stp === gameArray.value[a][x1] && x2 !== 0){
          gameArray.value[b][x2 === 4 ? 3 : x2 - 1] = gameArray.value[a][x1];
          gameArray.value[a][x1] = 0;
          x2-=1;
          if(x2 !== 0) x1+=1;
          }
          if (gameWin()) {
          setTimeout(() => {
              level++;
              levelUp()
          }, 500)
          }
      }
    }
    const enW = (arr) => {
      for (let i = 0; i < arr.length; i++) {
        if (arr[i] !== 0) {
        return i;
        }
      }
      return 4;
    }
    const gameWin = () => {
      let summ = 0;
      for (let i = 0; i < gameArray.value.length; i++) {
        for (let j = 0; j < 4; j++) {
        if (gameArray.value[i][0] == gameArray.value[i][j]) {
            summ++;
        }
      }
    }
    if (summ == gameArray.value.length * 4) return true;
      return false;
    }
    onBeforeMount(levelUp);
    return {
      gameArray,
      level,
      BottleHandler,
      colorPick,
      bottleFull
    }
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}
.container {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  background: #111;
}
.container h1{
  margin-bottom: 40px;
  color: #fff;
  font-family: 'Courier New', Courier, monospace;
}
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 30px;
  max-width: 350px;
}
.bottle {
  position: relative;
  height: 150px;
  width: 40px;
  border: 2px solid #fff;
  border-radius: 0 0 25px 25px;
  display: flex;
  justify-content: flex-end;
  cursor: pointer;
  flex-direction: column;
  align-items: center;
  transition: 0.3s;
  transform: translateY(0px);
}
.bottle::before {
  content: "";
  position: absolute;
  background: linear-gradient(#fffc, transparent);
  width: 5px;
  height: 100px;
  top: 20px;
  left: 5px;
  border-radius: 5px 5px 0 0;
  z-index: 2;
}
.bottle::after {
  content: "";
  position: absolute;
  /* background-color: #fff; */
  box-shadow: inset -3px 0 0 3px #0004;
  width: 100%;
  height: 100%;
  top: 0;
  z-index: 2;
  right: 0;
  border-radius: 0 0 25px 25px;
}
.bottle.isHand {
  transform: translateY(-20px);
}
.color {
  position: relative;
  width: 100%;
  height: 33px;
}
.color-enter-active,
.color-leave-active {
  transition: all 0.3s ease;
}

.color-enter-from,
.color-leave-to {
  opacity: 0;
  height: 0;
}
.color-move{
  transition: height 0.4s ease, opacity 0.4s ease, transform 0;
}
.color:last-child {
  border-radius: 0 0 25px 25px;
}
.color.red {
  background: red;
}
.color.blue {
  background: blue;
}
.color.green {
  background: lime;
}
.color.yellow {
  background: yellow;
}
.color.orange {
  background: orange;
}
.color.purple {
  background: purple;
}
.color.brown {
  background: saddlebrown;
}
.color.pink{
  background: pink;
}
.color.green{
  background: green;
}
.color.deeppink{
  background: deeppink;
}
.bottle.isSwap {
  animation: swaping 2s linear forwards;
}
@keyframes swaping {
  0%,
  100% {
    top: 0;
    left: 0;
  }
  40% {
    top: calc(1px * var(--y));
    left: calc(1px * var(--x));
    /* transform: translate(calc(1px*var(--x)),
        calc(1px*var(--x))); */
    transform-origin: 0 center;
  }
  30%,
  70% {
    top: calc(1px * var(--y));
    left: calc(1px * var(--x));
    transform:
         /* translate(calc(1px*var(--x)),
        calc(1px*var(--x))) */ rotate(
      0
    );
  }
  50% {
    top: calc(1px * var(--y));
    left: calc(1px * var(--x));
    transform: 
        /* translate(calc(1px*var(--x)),
        calc(1px*var(--x)))  */ rotate(
      -85deg
    );
    transform-origin: center center;
  }
}
</style>