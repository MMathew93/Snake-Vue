<template>
  <div class="gameBoard">
    <canvas id="canvas"></canvas>
  </div>
</template>

<script>
  export default {
    name: 'Game',
    data() {
      return {
        snake: [0, 1, 2]
      }
    },
    methods: {
      //fix the canvas dpi for screen
      fix_dpi() {
        let dpi = window.devicePixelRatio;
        let canvas = document.getElementById('canvas');
        //get CSS height
        //the + prefix casts it to an integer and the slice method gets rid of "px"
        let style_height = +getComputedStyle(canvas).getPropertyValue("height").slice(0, -2);
        //get CSS width
        let style_width = +getComputedStyle(canvas).getPropertyValue("width").slice(0, -2);
        //scale the canvas
        canvas.setAttribute('height', style_height * dpi);
        canvas.setAttribute('width', style_width * dpi);
      },
    //draw the game
    draw() {
      const canvas = document.getElementById('canvas');
      const snake = canvas.getContext('2d');
      snake.fillStyle = 'green'
      for(let i = 0; i < this.snake.length; i++ ) {
        snake.fillStyle = 'green'
        snake.fillRect([i].x, [i].y, 20, 20)
      }

      setInterval(this.draw, 1000)
    },

    //directions
    directions(event) {
      //event.keyCode 37 - 40
      if(event.keyCode == 37) {
        console.log('hello')
      } else {
        console.log('false')
      }
    }

    },
    mounted() {
      //draw the snake
      this.fix_dpi()
      const canvas = document.getElementById('canvas');
      const snake = canvas.getContext('2d');
      snake.fillStyle = 'green'
      snake.fillRect(100, 100, 20, 20);
      document.addEventListener('keydown', this.directions())
    }
  }
</script>

<style scoped>
  #canvas {
    width: 99vmin;
    height: 99vmin;
    border: 1px solid black;
    background: gray;
  }
</style>