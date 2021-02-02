<template>
  <!-- <div class="stage" @click="toggleAnimation"></div> -->
  <div id="stage" ref="root"  @click="toggleAnimation"></div>
  <!-- <div id="stage" ref="root"></div> -->
</template>

<script>
import * as PIXI from "pixi.js";
import shaderCode from "!raw-loader!../shader.txt";

export default {
  name: "Canvas",
  props: {
    seed: Number,
    pause: Boolean,
    triggerScreenshot: Boolean,
  },
  data() {
    return {
      app: null,
      shaderCode,
      sprite:null,
      shader:null,
      count:0,
      threshold:0.45,
      tickerPause:false,
      screenshotBool:false,
      window_length : 100,
    };
  },
  mounted() {
    this.$refs.root.addEventListener('wheel', this.onScroll);
    window.addEventListener('keydown', (e) => {
      switch (e.key) {
        case ' ':
          this.toggleAnimation();
          break;
        case 'Left':
        case 'ArrowLeft':
          if (this.tickerPause){
            this.count-=0.05;
          }
          break;
        case 'Right':
        case 'ArrowRight':
          if (this.tickerPause){
            this.count+=0.05;
          }
          break;
        default:
          break;
      }
    });

    // square canvas
    if ( window.innerWidth < window.innerHeight ){
      this.window_length = window.innerWidth; 
      this.app = new PIXI.Application({width: window.innerWidth, height: window.innerWidth});
    }else{
      this.window_length = window.innerHeight; 
      this.app = new PIXI.Application({width: window.innerHeight, height: window.innerHeight});
    }
    this.app.renderer.autoResize = true;
    this.app.renderer.clear();

    this.$refs.root.appendChild(this.app.view);

    this.sprite = new PIXI.Sprite();
    this.sprite.width = 512; 
    this.sprite.height = 512;

    var uniforms = {
      time : 0.0,
      seed : 0.0,
      threshold : 0.45,
      windowSize: [window.innerWidth, window.innerHeight],
      spriteSize: [256, 256],
    }
  
    this.shader = new PIXI.Filter(null, shaderCode, uniforms);
    this.shader.uniforms.time = 1.0;
    this.shader.uniforms.seed = 0.0;
        this.shader.uniforms.threshold = 0.45;
    this.shader.uniforms.windowSize = [window.innerWidth, window.innerHeight];
    this.shader.uniforms.spriteSize = [this.sprite.width, this.sprite.height];

    // Add shader to whatever is rendered by the container
    this.sprite.filters = [this.shader];

    var container = new PIXI.Container();
    container.addChild(this.sprite);
    this.app.stage.addChild(container);
    container.scale.set(this.window_length, this.window_length);

    this.count = this.shader.uniforms.seed;
    this.app.ticker.add(() => {
      if(this.screenshotBool){
        this.takeScreenshot();
        this.screenshotBool = !this.screenshotBool;
      }
      if(this.tickerPause == false){
        this.count += 0.01;
      }
      this.shader.uniforms.time = this.count;
      this.shader.uniforms.threshold = this.threshold;
    });
  },
  methods: {
    toggleAnimation(){
      if (this.tickerPause){
        console.log("time: " + this.count);
      }
      this.tickerPause = !this.tickerPause;
    },
    onScroll(event){
      if(this.tickerPause == true){
        if (event.deltaX < 0){
            this.count+=0.05;
        }else if (event.deltaX > 0){
          this.count-=0.05;
        }
      }
    },
    fadeCanvas(){
      console.log("fading out..");
      this.tickerPause = true;
      var thresh_int = this.threshold * 1000;
      var inc = -2;
      var that = this;
      var fadeThreshold = setInterval(function(){
          thresh_int += inc;
          that.threshold = thresh_int/1000;
          if(thresh_int <= 250) {
            that.count = that.seed;
            that.shader.uniforms.seed = that.seed;
            inc = 2;
            console.log("fading in..");
          }
          if(thresh_int >= 450) {
            that.tickerPause = false;
            clearInterval(fadeThreshold);
          }
      }, 10);  
    },
    takeScreenshot(){
      if(!this.tickerPause){
        this.toggleAnimation();
      }

      // Use hack for v5 https://github.com/pixijs/pixi.js/wiki/v5-Hacks#extract
      const renderTexture = PIXI.RenderTexture.create({width:window.innerHeight, height:window.innerHeight}); // enter your size , maybe from app.screen
      this.app.renderer.render(this.app.stage, renderTexture);
      var img = this.app.renderer.extract.canvas(renderTexture)
      
      // Flip vertically because of Pixijs library code flipping it
      var canvasContext = img.getContext('2d');
      canvasContext.translate(0, this.window_length);
      canvasContext.scale(1, -1);
      canvasContext.drawImage(img, 0, 0);

      img.toBlob((b) => {
          const a = document.createElement('a');
          document.body.append(a);
          a.download = 'rorschach';
          a.href = URL.createObjectURL(b);
          a.click();
          a.remove();
      }, 'image/png');
    }
  },
  watch:{
    seed(){
      this.fadeCanvas();
    },
    triggerScreenshot(){
      this.screenshotBool = !this.screenshotBool;
    }
  }
};
</script>

<style>
#stage{
  height:100%;
  /* width:100%; */
  /* overflow-y:hidden !important; */

}
</style>
