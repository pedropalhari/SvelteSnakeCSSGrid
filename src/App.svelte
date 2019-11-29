<script>
  export let name;

  let snakeBody = [
    { x: 7, y: 10 },
    { x: 8, y: 10 },
    { x: 9, y: 10 },
    { x: 10, y: 10 }
  ];

  let apple = {
    x: Math.floor(Math.random() * 20),
    y: Math.floor(Math.random() * 20)
  };

  let directionFacing = "ArrowRight";
  let changedDirection = false;
  function handleKeydown(event) {
    if (changedDirection) return;
    if (event.key == "ArrowRight" && directionFacing != "left")
      directionFacing = "right";

    if (event.key == "ArrowLeft" && directionFacing != "right")
      directionFacing = "left";

    if (event.key == "ArrowUp" && directionFacing != "down")
      directionFacing = "up";

    if (event.key == "ArrowDown" && directionFacing != "up")
      directionFacing = "down";

    changedDirection = true;
  }

  function mainloop() {
    console.log("apple", apple);

    let bodyToPush = {
      x: snakeBody[snakeBody.length - 1].x + 1,
      y: snakeBody[snakeBody.length - 1].y
    };

    if (directionFacing == "up")
      bodyToPush = {
        x: snakeBody[snakeBody.length - 1].x,
        y: snakeBody[snakeBody.length - 1].y - 1
      };

    if (directionFacing == "right")
      bodyToPush = {
        x: snakeBody[snakeBody.length - 1].x + 1,
        y: snakeBody[snakeBody.length - 1].y
      };

    if (directionFacing == "down")
      bodyToPush = {
        x: snakeBody[snakeBody.length - 1].x,
        y: snakeBody[snakeBody.length - 1].y + 1
      };

    if (directionFacing == "left")
      bodyToPush = {
        x: snakeBody[snakeBody.length - 1].x - 1,
        y: snakeBody[snakeBody.length - 1].y
      };

    //Condição ressetar
    if (
      bodyToPush.x >= 21 ||
      bodyToPush.y >= 21 ||
      bodyToPush.x <= 0 ||
      bodyToPush.y <= 0 ||
      snakeBody.find(body => body.x == bodyToPush.x && body.y == bodyToPush.y)
    ) {
      snakeBody = [
        { x: 7, y: 10 },
        { x: 8, y: 10 },
        { x: 9, y: 10 },
        { x: 10, y: 10 }
      ];
      return;
    }

    snakeBody.push(bodyToPush);

    //Condição comer maçã
    if (bodyToPush.x == apple.x && bodyToPush.y == apple.y) {
      apple = {
        x: Math.floor(Math.random() * 20),
        y: Math.floor(Math.random() * 20)
      };
    } else snakeBody.shift();

    changedDirection = false;

    snakeBody = snakeBody;
  }

  setInterval(() => {
    mainloop();
  }, 300);
</script>

<style>
  #rootBody {
    width: 100vw;
    height: 100vh;

    display: flex;
    justify-content: center;
    align-items: center;
  }

  #rootGrid {
    display: grid;
    grid-template-columns: repeat(20, 20px);
    grid-template-rows: repeat(20, 20px);
    grid-gap: 5px;

    border-color: black;
    border-width: 2px;
    border-style: solid;
    padding: 3px;
  }

  .snakePart {
    background-color: black;
  }

  #apple {
    background-color: red;
  }
</style>

<svelte:window on:keydown={handleKeydown} />

<div id="rootBody">
  <div id="rootGrid">

    {#each snakeBody as snake}
      <div
        class="snakePart"
        style="grid-column: {snake.x} / {snake.x + 1}; grid-row: {snake.y} / {snake.y + 1};" />
    {/each}

    <div
      id="apple"
      style="grid-column: {apple.x} / {apple.x + 1}; grid-row: {apple.y} / {apple.y + 1};" />
  </div>
</div>
