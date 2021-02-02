<template>
  <div id="app">
    <div id="background"/>
    <div id="randomize-container">
      <p class="h1 mb-2"><b-icon icon="dice6" id="randomize-button" @click="generateSeed()"></b-icon></p>
      <h4>#{{seed}}</h4>
    </div>
    <div id="right-container">
      <p class="h1 mb-2"><b-icon icon="download" id="screenShot-button" @click="screenShotBool = !screenShotBool"></b-icon></p>
      <a href="https://github.com/andibakti">
        <svg id="github-icon" width="32px" height="32px" viewBox="0 0 16 16" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <title>GitHub</title>
          <path id="path-style" d="M8,0.2c-4.4,0-8,3.6-8,8c0,3.5,2.3,6.5,5.5,7.6
          C5.9,15.9,6,15.6,6,15.4c0-0.2,0-0.7,0-1.4C3.8,14.5,3.3,13,3.3,13c-0.4-0.9-0.9-1.2-0.9-1.2c-0.7-0.5,0.1-0.5,0.1-0.5
          c0.8,0.1,1.2,0.8,1.2,0.8C4.4,13.4,5.6,13,6,12.8c0.1-0.5,0.3-0.9,0.5-1.1c-1.8-0.2-3.6-0.9-3.6-4c0-0.9,0.3-1.6,0.8-2.1
          c-0.1-0.2-0.4-1,0.1-2.1c0,0,0.7-0.2,2.2,0.8c0.6-0.2,1.3-0.3,2-0.3c0.7,0,1.4,0.1,2,0.3c1.5-1,2.2-0.8,2.2-0.8
          c0.4,1.1,0.2,1.9,0.1,2.1c0.5,0.6,0.8,1.3,0.8,2.1c0,3.1-1.9,3.7-3.7,3.9C9.7,12,10,12.5,10,13.2c0,1.1,0,1.9,0,2.2
          c0,0.2,0.1,0.5,0.6,0.4c3.2-1.1,5.5-4.1,5.5-7.6C16,3.8,12.4,0.2,8,0.2z"/>
        </svg>
      </a> 
    </div>
    <div id="socials-container">
    </div>

    <Canvas id="canvas" :seed="seed" :triggerScreenshot="screenShotBool"/>

    <!-- <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link> -->
    <router-view></router-view>
  </div>
</template>

<script>
import Canvas from "./components/Canvas";
import shaderCode from "!raw-loader!./shader.txt";

export default {
  name: 'App',
  components: {
    Canvas
  },
  data(){
    return {
      shaderCode,
      seed : 0,
      screenShotBool : false,
      pause : false
      };
  },
  mounted(){
    var parse = parseInt(this.$route.query.seed);
    if (!isNaN(parse)){
      this.seed = parse;
    }


    window.addEventListener('keydown', (e) => {
      if (e.key == 'Enter') {
        if (document.fullscreenElement != null){
          document.exitFullscreen()
            .catch((err) => console.error(err))
        }else{
          this.$el.requestFullscreen()
            .catch((err) => console.error(err))
        }
      }
    });
  },
  methods:{
    generateSeed(){
      this.seed = Math.floor(32768*(Math.random()));
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #4e4e4e;
  
  overflow:hidden !important;
  height:100%;
  width:100%;
}

#canvas-container{
  height:100%;
  width:100%;
}

#canvas{
  position:fixed;
  /* z-index: -1; */
  z-index: 0;

  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

#randomize-container{
  z-index: 2;
  color : rgb(170, 170, 170);
  position:fixed;
  top:  3%;
  left: 3%;
  cursor: pointer;
}

#randomize-container:hover{
  color: #3f3f3f;
}

#right-container{
  z-index: 2;
  position:fixed;
  top:  3%;
  right: 3%;
}

#screenShot-button{
  cursor: pointer;
  color : rgb(170, 170, 170);
}
#screenShot-button:hover{
  color: #3f3f3f;
}
/* #socials-container{
  z-index: 2;
  color : rgb(170, 170, 170);
  position:fixed;

  top:  6%;
  right: 3%;
}
#socials-container:hover{
  color: #3f3f3f;
} */

#github-icon{
  /* width:90%; */
  height:auto;

}

#path-style{
  fill : rgb(170, 170, 170);
}
#path-style:hover{
  fill : #3f3f3f;
}

#background{
  align-items: center;
  display:flex;
  position:fixed;
  /* background: #cacaca; */
  height:100%;
  width:100%;
  top: 0%;
  left: 0%;
  z-index: -1;

}
</style>
