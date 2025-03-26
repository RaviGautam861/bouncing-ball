<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Catch the Falling Stars</title>
    <style>
        body {
            text-align: center;
            background-color: black;
            color: white;
            font-family: Arial, sans-serif;
        }
        canvas {
            background: lightblue;
            display: block;
            margin: 20px auto;
        }
    </style>
</head>
<body>
    <h1>Catch the Falling Stars</h1>
    <canvas id="gameCanvas" width="500" height="600"></canvas>
    <p>Use Left & Right Arrow Keys to Move</p>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const basket = { x: 225, y: 550, width: 50, height: 20, speed: 10 };
        const stars = [];
        let score = 0;

        function createStar() {
            stars.push({ x: Math.random() * 450, y: 0, size: 10, speed: 2 + Math.random() * 3 });
        }

        function drawBasket() {
            ctx.fillStyle = "brown";
            ctx.fillRect(basket.x, basket.y, basket.width, basket.height);
        }

        function drawStars() {
            ctx.fillStyle = "yellow";
            stars.forEach(star => {
                ctx.beginPath();
                ctx.arc(star.x, star.y, star.size, 0, Math.PI * 2);
                ctx.fill();
            });
        }

        function moveStars() {
            stars.forEach(star => {
                star.y += star.speed;
            });

            for (let i = stars.length - 1; i >= 0; i--) {
                if (stars[i].y > canvas.height) {
                    stars.splice(i, 1);
                }
            }
        }

        function checkCollision() {
            stars.forEach((star, index) => {
                if (
                    star.y + star.size >= basket.y &&
                    star.x >= basket.x &&
                    star.x <= basket.x + basket.width
                ) {
                    score++;
                    stars.splice(index, 1);
                }
            });
        }

        function drawScore() {
            ctx.fillStyle = "white";
            ctx.font = "20px Arial";
            ctx.fillText("Score: " + score, 20, 30);
        }

        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBasket();
            drawStars();
            moveStars();
            checkCollision();
            drawScore();
            requestAnimationFrame(gameLoop);
        }

        document.addEventListener("keydown", (event) => {
            if (event.key === "ArrowLeft" && basket.x > 0) {
                basket.x -= basket.speed;
            } else if (event.key === "ArrowRight" && basket.x < canvas.width - basket.width) {
                basket.x += basket.speed;
            }
        });

        setInterval(createStar, 1000);
        gameLoop();
    </script>
</body>
</html>
