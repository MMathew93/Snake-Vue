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
        snake: [{
            x: 9 * 20,
            y: 10 * 20
          },
          {
            x: 8 * 20,
            y: 10 * 20
          },
          {
            x: 7 * 20,
            y: 10 * 20
          }
        ],
        direction: '',
        startGame: false
      }
    },

    watch: {
      startGame(value) {
        if (value) {
          this.draw()
        }
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
        const context = canvas.getContext('2d');
        for (let i = 0; i < this.snake.length; i++) {
          context.fillStyle = ( i == 0 ) ? "green" : "gray";
          context.fillRect(this.snake[i].x, this.snake[i].y, 20, 20)
        }
        //head of the snake
        let snakeX = this.snake[0].x;
        let snakeY = this.snake[0].y;

        if (this.direction === 'left') {
          snakeX -= 20
        }
        if (this.direction === 'up') {
          snakeY -= 20
        }
        if (this.direction === 'right') {
          snakeX += 20
        }
        if (this.direction === 'down') {
          snakeY += 20
        }

        //remove tail
        this.snake.pop();

        //add new head
        let newHead = {
          x: snakeX,
          y: snakeY
        }
        this.snake.unshift(newHead);


        setInterval(this.draw, 1000)
      },

      //directions
      directions(event) {
        this.startGame = true
        //event.keyCode 37 - 40
        if (event.keyCode == 37) {
          this.direction = 'left'
        } else if (event.keyCode == 38) {
          this.direction = 'up'
        } else if (event.keyCode == 39) {
          this.direction = 'right'
        } else if (event.keyCode == 40) {
          this.direction = 'down'
        }
      },
    },
    mounted() {
      //draw the snake
      this.fix_dpi()
      document.addEventListener('keydown', this.directions)
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