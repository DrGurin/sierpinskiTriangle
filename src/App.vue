<template>
  <div id="app">
    <div v-on:mousemove="getCoordinate" @click="stopDrag" class="content_up">
          <button @click="showControlls()" id="show-controlls" class="pa button open-controlls open-controlls-fake">{{statusOfControls[0]}} controlls</button>
      <div v-if="showFirstTop" :style="getPositionFirstTop" class="top pa top_first">
        <p class="letter">A</p>
      </div>
      <div v-if="showSecondTop" :style="getPositionSecondTop" class="top pa top_second">
        <p class="letter">B</p>
      </div>
      <div v-if="showThirdTop" :style="getPositionThirdTop" class="top pa top_third">
        <p class="letter">C</p>
      </div>
      <div v-if="showStartTop" :style="getPositionStartTop" class="top pa top_start"></div>
      <div class="it-dot pa" v-for="(dot, index) in dots" :key="index" :style="dot"></div>
      <div v-if="showFirstTop&&showSecondTop&&showThirdTop" :style="getWidthFirst" :class="[isTopChangable ? '' :'shadow']" class="line pa line_first"></div>
      <div v-if="showFirstTop&&showSecondTop&&showThirdTop" :style="getWidthSecond" :class="[isTopChangable ? '' :'shadow']" class="line pa line_second"></div>
      <div v-if="showFirstTop&&showSecondTop&&showThirdTop" :style="getWidthThird" :class="[isTopChangable ? '' :'shadow']" class="line pa line_third"></div>
    </div>
    <div v-bind:class="[ isControllsOpened ? 'content_down-show' : 'content_down-hide', '']" class="content_down">
          <button @click="showControlls()" id="show-controlls" class="pa button open-controlls">{{statusOfControls[0]}} controlls</button>
      <div class="data_status">
        <p class="iterator">Quantity of iterations: {{dots.length}}</p>
      </div>
      <div class="button_wrapper">
        <div class="subwrapper set_tops">
          <button @click="setTopFirst()" id="setTopFirst" class="button set_top set_top_first">Set first top</button>
          <button @click="setSecondTop()" id="setTopSecond" class="button set_top set_top_second">Set second top</button>
          <button @click="setThirdTop()" id="setTopThird" class="button set_top set_top_third">Set third top</button>
          <button @click="setStartTop()" id="setTopStart" class="button set_top set_top_start">Set start top</button><br>
          <button @click="resetTops()" class="button reset_top">RESET</button>
          <button @click="createTriangle()" class="button create_triangle">Create triangle</button>
          <button @click="startInterval()" id="stopInterval" class="button stop_interval">Start iteration</button><br>
          <button @click="stopInterval()" id="stopInterval" class="button stop_interval">Stop iteration</button><br>
        </div>

        <!-- <div class="subwrapper reset">
          <button @click="resetTops()" class="button reset_top">RESET</button>
        </div>
        <div class="subwrapper create">
          <button @click="createTriangle()" class="button create_triangle">Create triangle</button>
        </div>
        <div class="subwrapper starter">
          <button @click="startInterval()" id="stopInterval" class="button stop_interval">Start iteration</button><br>
        </div>
        <div class="subwrapper stoper">
          <button @click="stopInterval()" id="stopInterval" class="button stop_interval">Stop iteration</button><br>
        </div>       -->
      </div>
    </div>
  </div>
</template>

<script>
import { log } from 'util';
// ||showSecondTop||showThirdTop

export default {
  name: 'app',
  data() {
    return{
      isTopChangable: true, 
      canSetCoordinate: false, 
      isControllsOpened: false, 
      statusOfControls: [
        'Show'
      ],
      showFirstTop: false,
      dragableFirst: false,
      showSecondTop: false, 
      dragableSecond: false,
      showThirdTop: false, 
      dragableThird: false,
      showStartTop: false,
      dragableStart: false,
      firstTop: 0,
      firstLeft: 0,
      secondTop: 0,
      secondLeft: 0,
      thirdTop: 0,
      thirdLeft: 0,
      lengthFirst: 0,
      firstAngle: 0,
      lengthSecond: 0,
      secondAngle: 0,
      lengthThird: 0,
      thirdAngle: 0, 
      dots: [

      ],
      newDot: null,
      newTop: 0,
      newLeft: 0
    }
  },
  methods: {
    showControlls() {
      this.isControllsOpened = !this.isControllsOpened;
      this.statusOfControls.splice(1, 1);
      if(this.isControllsOpened){
        this.statusOfControls.unshift('Hide')
      } else {
        this.statusOfControls.unshift('Show')
      }
    },
    randomTop(){
      let randTop = 1-0.5 + Math.random()*(3-1 +1); 
      return Math.round(randTop);
    }, 
    setCoordinates() {
      let random = this.randomTop()
      if(random == 1 && this.canSetCoordinate) {
        // let top = this.newTop
        // let left = this.newLeft
        this.newTop = (this.firstTop + this.newTop)*0.5
        this.newLeft = (this.firstLeft + this.newLeft)*0.5
        this.newDot = `top: ${this.newTop}px; left: ${this.newLeft}px`;
        this.dots.push(this.newDot);
      } else if(random == 2 && this.canSetCoordinate) {
        // let top = this.newTop
        // let left = this.newLeft
        this.newTop = (this.secondTop + this.newTop)*0.5 
        this.newLeft = (this.secondLeft + this.newLeft)*0.5 
        this.newDot = `top: ${this.newTop}px; left: ${this.newLeft}px`;
        this.dots.push(this.newDot);
      } else if(random == 3 && this.canSetCoordinate) {
        // let top = this.newTop
        // let left = this.newLeft
        this.newTop = (this.thirdTop + this.newTop)*0.5
        this.newLeft = (this.thirdLeft + this.newLeft)*0.5
        this.newDot = `top: ${this.newTop}px; left: ${this.newLeft}px`;
        this.dots.push(this.newDot);
      }
    }, 
    stopDrag(event) {
      if(this.dragableFirst == true) {
        this.dragableFirst = false; 
        this.dragableSecond = false; 
        this.dragableThird = false; 
        this.dragableStart = false;
        this.firstTop = event.clientY;
        this.firstLeft = event.clientX; 
      }
      else if(this.dragableSecond == true) {
        this.dragableFirst = false; 
        this.dragableSecond = false; 
        this.dragableThird = false; 
        this.dragableStart = false;
        this.secondTop = event.clientY;
        this.secondLeft = event.clientX;
      }
      else if(this.dragableThird == true) {
        this.dragableFirst = false; 
        this.dragableSecond = false; 
        this.dragableThird = false;
        this.dragableStart = false; 
        this.thirdTop = event.clientY;
        this.thirdLeft = event.clientX;  
      }
      else if(this.dragableStart == true) {
        this.dragableFirst = false; 
        this.dragableSecond = false; 
        this.dragableThird = false;
        this.dragableStart = false;
        this.newTop = event.clientY;
        this.newLeft = event.clientX; 
      }
    },
    getCoordinate(event) {
      if(this.dragableFirst&&this.showFirstTop) {
         this.firstTop = event.clientY; 
         this.firstLeft = event.clientX; 
      }
      else if(this.dragableSecond&&this.showSecondTop) {
        this.secondTop = event.clientY;
        this.secondLeft = event.clientX; 
      }
      else if(this.dragableThird&&this.showThirdTop) {
        this.thirdTop = event.clientY;
        this.thirdLeft = event.clientX; 
      }
      else if(this.dragableStart&&this.showStartTop) {
        this.newTop = event.clientY;
        this.newLeft = event.clientX; 
      }
    },
    setTopFirst(event) {
      if(this.isTopChangable){
        this.showFirstTop = true; 
        this.dragableFirst = true; 
      }       
    },
    setSecondTop(event) { 
      if(this.isTopChangable){
        this.showSecondTop = true; 
        this.dragableSecond = true; 
      }
    },
    setThirdTop(event) {
      if(this.isTopChangable){
        this.showThirdTop = true; 
        this.dragableThird = true; 
      }
    },
    setStartTop(event) {
      if(!this.canSetCoordinate){
        this.showStartTop = true; 
        this.dragableStart = true; 
      }
    },
    resetTops() {
      this.canSetCoordinate = false; 
      this.showFirstTop = false; 
      this.showSecondTop = false; 
      this.showThirdTop = false;
      this.showStartTop = false;  
      this.dots = []; 
      this.firstAngle = 0; 
      this.secondAngle = 0; 
      this.thirdAngle = 0; 
      this.lengthFirst = 0;
      this.lengthSecond = 0;
      this.lengthThird = 0;
      this.isTopChangable = true;
    },
    createTriangle() {
      if(this.showFirstTop&&this.showSecondTop&&this.showThirdTop) {
        this.isTopChangable = false; 
        this.lengthFirst = Math.sqrt((this.firstTop-this.secondTop) * (this.firstTop-this.secondTop) + (this.firstLeft-this.secondLeft) * (this.firstLeft-this.secondLeft))
        this.firstAngle = Math.atan2(this.secondTop-this.firstTop, this.secondLeft-this.firstLeft) * 180 / Math.PI;
        this.lengthSecond = Math.sqrt((this.secondTop-this.thirdTop) * (this.secondTop-this.thirdTop) + (this.secondLeft-this.thirdLeft) * (this.secondLeft-this.thirdLeft))
        this.secondAngle = Math.atan2(this.thirdTop-this.secondTop, this.thirdLeft-this.secondLeft) * 180 / Math.PI;
        this.lengthThird = Math.sqrt((this.firstTop-this.thirdTop) * (this.firstTop-this.thirdTop) + (this.firstLeft-this.thirdLeft) * (this.firstLeft-this.thirdLeft))
        this.thirdAngle = Math.atan2(this.firstTop-this.thirdTop, this.firstLeft-this.thirdLeft) * 180 / Math.PI;
      }
      else{
        alert('Установите три вершины, для Вашего треугольника')
      }
    },
    startInterval() {
      this.canSetCoordinate = true; 
      setInterval(this.setCoordinates, 0.1); 
    },
    stopInterval() {
      this.canSetCoordinate = false; 
      // clearInterval(this.startInterval);
    }
  },
  computed: {
    getPositionFirstTop() {
      if(this.dragableFirst){
        return `top:${this.firstTop}px;left:${this.firstLeft}px`
      }
      else{
        return `top:${this.firstTop}px;left:${this.firstLeft}px`
      }
    },
    getPositionSecondTop() {
      if(this.dragableSecond){
        return `top:${this.secondTop}px;left:${this.secondLeft}px`
      }
      else{
        return `top:${this.secondTop}px;left:${this.secondLeft}px`
      }
    },
    getPositionThirdTop() {
      if(this.dragableThird){
        return `top:${this.thirdTop}px;left:${this.thirdLeft}px`
      }
      else{
        return `top:${this.thirdTop}px;left:${this.thirdLeft}px`
      }
    },
    getPositionStartTop() {
      if(this.dragableStart){
        return `top:${this.newTop}px;left:${this.newLeft}px`
      }
      else{
        return `top:${this.newTop}px;left:${this.newLeft}px`
      }
    },
    getWidthFirst() {
      return `width: ${this.lengthFirst}px; top:${this.firstTop+3}px; left:${this.firstLeft+5}px; transform: rotate(${this.firstAngle}deg); transformOrigin: 0 0`
    },
    getWidthSecond() {
      return `width: ${this.lengthSecond}px; top:${this.secondTop+3}px; left:${this.secondLeft+5}px; transform: rotate(${this.secondAngle}deg); transformOrigin: 0 0`
    },
    getWidthThird() {
      return `width: ${this.lengthThird}px; top:${this.thirdTop+3}px; left:${this.thirdLeft+5}px; transform: rotate(${this.thirdAngle}deg); transformOrigin: 0 0`
    }
  }
}
</script>

<style>
body{
  width: 100vw;
  height: 100%;
  min-width: 100vw;
  max-width: 100vw; 
  min-height: 100vh; 
}
*{
  box-sizing: border-box; 
  margin: 0; 
  padding: 0; 
  font-family: 'Helvetica', Helvetica, sans-serif; 
  -webkit-font-smoothing: antialiased; 
}
#app {
  height: 100vh;
  width: 100%; 
  overflow: hidden; 
  display: flex; 
  flex-direction: column;
  justify-content: space-between;
  background-color: darkslategray;
}
#app .line.shadow{
  -webkit-box-shadow: 0px 0px 5px 4px #000;  /* Safari 3-4, iOS 4.0.2 - 4.2, Android 2.3+ */
  -moz-box-shadow:    0px 0px 5px 4px #000;  /* Firefox 3.5 - 3.6 */
  box-shadow:         0px 0px 5px 4px #000; 
}
.content_up{
  width: 100%; 
  height: 90vh;
}
.content_down{
  width: 100vw;
  height: 10vh;
  border: 3px solid green;
  height: max-content;
  position: relative;
  box-shadow: #f96332; 
  box-shadow: 0px 0px 20px 0px rgba(0, 0, 0, 0.15);
  padding-top: 15px;
}
.button_wrapper{ 
  width: 100%;
  /* height: 50%;  */
  position: absolute; 
  display: flex; 
  flex-direction: column; 
  justify-content: center; 
  align-content: center; 
  bottom: 0;  
  padding: 20px;
  margin-bottom: 20px;
}
.button{
  width: 100px; 
  height: 50px;
  margin: 10px; 
  background: none; 
  border: 3px solid black; 
  color: black;
  border-radius: 5px; 
  font-weight: bold; 
  transition: 0.3s;
  min-width: 80px;
}
.subwrapper{
  display: flex; 
  flex-direction: row; 
  justify-content: center; 
  align-content: center; 
  flex-wrap: wrap;
}
#app .open-controlls{
  display: none;
  width: 100px;
  height: 50px;
  background: grey;
  top: -50px;
  left: 0;
  margin: 0 auto;
  border: 3px solid #635f5f;
  border-radius: 15px;
  right: 0;
}

@media (max-width: 1010px){
      .content_down{
        position: absolute; 
        height: fit-content; 
        bottom: -80%;
        z-index: 3; 
        background: linear-gradient(#f96332 0%, #000 80%);  
        /* max-height: 325px; */
        /* overflow-y: scroll; */
      }
      .content_down-show{
        bottom: 0; 
        
        /* animation-name: showCustomContolls; 
        animation-delay: 0; 
        animation-duration: 0.7s; 
        animation-fill-mode: forwards; 
        animation-timing-function: ease-in;  */
      }
      .content_down-hide{
        bottom: -80%; 
      }
      .button_wrapper{
        position: static; 
      } 
      .button{
        border: 3px solid white; 
        color: white;
      }
      #app .open-controlls{
        display: block; 
      }
       #app .open-controlls.open-controlls-fake{
         bottom: 0; 
         top: unset; 
       }
       #app .iterator{
         color: white; 
       }

}

.pa{
  position: absolute; 
}
.top{
  height: 8px;
  width: 8px;
  background: #ffffff;
  /* border: 10px solid black;  */
  border-radius: 50%; 
  border: 2px solid black; 
  display: flex; 
  justify-items: center; 
  align-content: center; 
  text-align: center; 
  background: red;
  z-index: 2;
}
.letter{
  display: block; 
  color: gold;

}
.line {
  background-color: black;
  height: 3px;
  border-radius: 6px;
}
/* .top_first::before{
  content: 'A';
  position: absolute; 
  right: 50px; 
  bottom: 50px; 
  font-size: 30px; 
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif; 
}
.top_second::before{
  content: 'B';
  font-size: 30px; 
   position: absolute; 
  right: 50px; 
  bottom: 50px; 
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif; 
}
.top_third::after{
  content: 'C';
  font-size: 30px; 
   position: absolute; 
  left: 50px; 
  bottom: 50px; 
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif; 
} */
.data_status{
  height: 50%;
}

.button{
  width: 100px; 
  height: 50px;
  margin: 10px; 
  background: none; 
  border: 3px solid black; 
  border-radius: 5px; 
  font-weight: bold; 
  /* transition: 0.3s;
   -webkit-transition:0.3s; */
}
.create_triangle:hover{
  background-color: #3e8e41;
  color: white;
}
.reset_top:hover{
  background-color: #635f5f;
  color: white;
}
.set_top:hover{
  background-color: #3e8e41;
  color: white;
}
.stop_interval:hover{
  background-color: #c21919;
  color: white;
}

.it-dot {
  width: 2px;
  height: 2px;
  background-color: black;
  border-radius: 50%;
}
.iterator{
  color: #000; 
  font-weight: 900; 
  width: 100%;
  text-align: center; 
}
</style>
