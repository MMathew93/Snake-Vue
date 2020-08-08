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
        snake: [{x: 100, y: 100}],
        direction: '',
        food: [],
        total: 1,
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

      drawSnake() {
        const canvas = document.getElementById('canvas');
        const context = canvas.getContext('2d');
        for (let i = 0; i < this.total; i++) {
          //colors the snake
          context.fillStyle = "green"
          context.fillRect(this.snake[i].x, this.snake[i].y, 20, 20)
        }
        context.fillRect(this.snake[0].x, this.snake[0].y, 20, 20)
      },
      
      spawnFood() {
        this.food = []
        let newFood = {
          x: (Math.floor(Math.random() * (1200 / 20) - 1) + 1) * 20,
          y: (Math.floor(Math.random() * (1200 / 20) - 1) + 1) * 20
        }
        this.food.push(newFood)
      },

      drawFruit() {
        const canvas = document.getElementById('canvas');
        const context = canvas.getContext('2d');
        context.fillStyle = "red";
        context.fillRect(this.food[0].x, this.food[0].y, 20, 20);
      },

      //directions
      directions(event) {
        this.startGame = true
        //event.keyCode 37 - 40
        if (event.keyCode == 37 && this.direction !== 'right') {
          this.direction = 'left'
        } else if (event.keyCode == 38 && this.direction !== 'down') {
          this.direction = 'up'
        } else if (event.keyCode == 39 && this.direction !== 'left') {
          this.direction = 'right'
        } else if (event.keyCode == 40 && this.direction !== 'up') {
          this.direction = 'down'
        }
      },

      collision(head, array) {
        for (let i = 0; i < array.length; i++) {
          if (head[0] === array[i][0] && head[1] === array[i][1]) {
            return true;
          }
        }
        return false;
      },

      snakeHeadPosition() {
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

        let newHead = {
          x: snakeX,
          y: snakeY
        }

        this.snake.unshift(newHead);
      },

      //draw the game
      draw() {
        const canvas = document.getElementById('canvas');
        const context = canvas.getContext('2d');
        context.clearRect(0, 0, canvas.width, canvas.width)
        this.drawSnake()
        this.snakeHeadPosition()
        this.drawFruit()
        //if snake eats the food do not remove tail, else do remove
        if (this.snake[0].x === this.food[0].x && this.snake[0].y === this.food[0].y) {
          this.total++
          for(let i = 0; i < this.snake.length; i++) {
            this.snake[i] = this.snake[i + 1]
          }
          this.snake[this.total - 1] = {x: this.x, y: this.y}
          this.spawnFood()
        } else {
          //remove tail
          this.snake.pop();
        }
      
        //game over // boundaries collision

        

        let game = setInterval(this.draw, 200)
        setTimeout(function () {
          clearInterval(game);
        }, 200)
      }
    },
    mounted() {
      //draw the snake
      this.fix_dpi()
      document.addEventListener('keydown', this.directions)
      this.drawSnake()
      this.spawnFood()
      this.drawFruit()
    }
  }
</script>

<style scoped>
  #canvas {
    width: 1200px;
    height: 1200px;
    border: 1px solid black;
    background: gray;
  }
</style>