# kenanslibrary
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <title>Snake Game</title>
    <style>
        body {
            background: #222;
            color: white;
            text-align: center;
            font-family: Arial, sans-serif;
        }
        canvas {
            background: #111;
            border: 2px solid #555;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Snake 🐍</h1>
    <canvas id="game" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById("game");
        const ctx = canvas.getContext("2d");

        let box = 20;
        let snake = [{ x: 9 * box, y: 9 * box }];
        let direction = "RIGHT";

        let food = {
            x: Math.floor(Math.random() * 20) * box,
            y: Math.floor(Math.random() * 20) * box
        };

        document.addEventListener("keydown", event => {
            if (event.key === "ArrowUp" && direction !== "DOWN") direction = "UP";
            if (event.key === "ArrowDown" && direction !== "UP") direction = "DOWN";
            if (event.key === "ArrowLeft" && direction !== "RIGHT") direction = "LEFT";
            if (event.key === "ArrowRight" && direction !== "LEFT") direction = "RIGHT";
        });

        function draw() {
            ctx.clearRect(0, 0, 400, 400);

            // Food
            ctx.fillStyle = "red";
            ctx.fillRect(food.x, food.y, box, box);

            // Snake
            ctx.fillStyle = "lime";
            snake.forEach(part => ctx.fillRect(part.x, part.y, box, box));

            // Movement
            let head = { ...snake[0] };
            if (direction === "UP") head.y -= box;
            if (direction === "DOWN") head.y += box;
            if (direction === "LEFT") head.x -= box;
            if (direction === "RIGHT") head.x += box;

            // Game Over
            if (
                head.x < 0 || head.x >= 400 ||
                head.y < 0 || head.y >= 400 ||
                snake.some(part => part.x === head.x && part.y === head.y)
            ) {
                alert("Game Over!");
                document.location.reload();
            }

            snake.unshift(head);

            // Eat food
            if (head.x === food.x && head.y === food.y) {
                food = {
                    x: Math.floor(Math.random() * 20) * box,
                    y: Math.floor(Math.random() * 20) * box
                };
            } else {
                snake.pop();
            }
        }

        setInterval(draw, 100);
    </script>
</body>
</html>
