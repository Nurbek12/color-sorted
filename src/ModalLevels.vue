<template>
  <div class="modal" @click="$emit('closeModal')">
    <div class="levels" @click.stop>
      <div v-for="(l,i) in levels" 
      :key="i" :class="`level ${i+1 == currentLevel ? 'current' : ''} ${i+1 <= endLevel ? 'open' : ''}`" 
      @click="changeLevel(i+1)"
      >{{i+1}}</div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'Modal',
    props: ['levels','currentLevel'],
    data(){
      return{
        endLevel: null
      }
    },
    created(){
      this.endLevel = localStorage.getItem('endlevel')
    },
    methods: {
      changeLevel(n){
        if(n > this.endLevel) return;
        if(n === this.currentLevel){
          this.$emit('closeModal');
          return;
        }
        this.$emit('changeLevel', n)
      }
    }
}
</script>

<style>
.modal{
  position: absolute;
  z-index: 11;
  height: 100%;
  width: 100%;
  background: #0008;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  animation: modal .1s linear forwards;
}
.levels{
  max-width: 430px;
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  background: #222;
  justify-content: flex-start;
}
@keyframes modal{
  0%{
    opacity: 0;
  }
  100%{
    opacity: 1;
  }
}
.level{
  position: relative;
  width: 50px;
  height: 50px;
  border-radius: 5px;
  background: rgb(255, 148, 82);
  margin: 10px;
  display: flex;
  justify-content: center;
  cursor: pointer;
  align-items: center;
  font-size: 1.2rem;
  color: #fff;
  font-family: monospace;
  font-weight: bolder;
  text-shadow: 0 1px 3px #0007;
  opacity: .4;
}
.level:after{
  position: absolute;
  content: '';
  width: 80%;
  height: 80%;
  border-radius: 5px;
  border: 1px solid #fff;
}
.level.open{
  opacity: 1;
}
.level.current{
  opacity: 1;
  background: rgb(255, 238, 0);
}
.level.current::after{
  border: 1px solid red;
}
</style>
