<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>TANKASS - Trash Sorting Game</title>
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }

      body,
      html {
        width: 100%;
        height: 100%;
        overflow-y: auto;
      }

      body {
        font-family: "Arial", sans-serif;
        background-color: #e0f7fa;
        display: flex;
        justify-content: center;
        align-items: flex-start;
      }

      #game-container {
        position: relative;
        width: 100%;
        min-height: 100vh;
        background-color: white;
      }

      /* Start Screen */
      #start-screen {
        position: relative;
        width: 100%;
        min-height: 100vh;
        background: linear-gradient(
            rgba(0, 121, 107, 0.8),
            rgba(0, 137, 123, 0.8)
          ),
          url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600"><rect width="800" height="600" fill="%2300796b"/><circle cx="200" cy="150" r="80" fill="%23ffd54f"/><path d="M100,400 Q200,300 300,400 T500,400 T700,400" stroke="%2381c784" stroke-width="20" fill="none"/><rect x="150" y="350" width="30" height="150" fill="%235d4037"/><path d="M165,350 Q165,250 180,250 Q195,250 195,350" fill="%232e7d32"/><rect x="450" y="380" width="30" height="120" fill="%235d4037"/><path d="M465,380 Q465,280 480,280 Q495,280 495,380" fill="%232e7d32"/></svg>')
            center/cover no-repeat;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: white;
        text-align: center;
        z-index: 100;
        padding: 40px 20px;
      }

      #start-content {
        background-color: rgba(255, 255, 255, 0.9);
        padding: 30px;
        border-radius: 15px;
        width: 90%;
        max-width: 800px;
        color: #00796b;
        box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
        margin: 40px 0;
      }

      /* Game Screen */
      #game-screen {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        display: none;
        overflow: hidden;
      }

      /* End Screen */
      #end-screen {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background-color: rgba(0, 121, 107, 0.9);
        display: none;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: white;
        text-align: center;
        z-index: 100;
      }

      h1 {
        color: #ffeb3b;
        margin-bottom: 10px;
        text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        font-size: 3rem;
      }

      h2 {
        color: #b2dfdb;
        margin-bottom: 20px;
        font-size: 1.8rem;
      }

      input,
      button {
        padding: 12px 20px;
        margin: 15px;
        border-radius: 8px;
        border: none;
        font-size: 1.2rem;
        width: 80%;
        max-width: 400px;
      }

      input {
        border: 2px solid #b2dfdb;
      }

      button {
        background-color: #ffc107;
        color: #00796b;
        font-weight: bold;
        cursor: pointer;
        transition: all 0.3s;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      }

      button:hover {
        background-color: #ffd54f;
        transform: translateY(-3px);
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
      }

      #hud {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        display: flex;
        justify-content: space-around;
        padding: 15px;
        background-color: rgba(178, 223, 219, 0.9);
        z-index: 10;
        font-size: 1.2rem;
        font-weight: bold;
        color: #00796b;
      }

      #play-area {
        position: absolute;
        width: 100%;
        height: 100%;
        overflow: hidden;
        background-image: url(PICTURES/ChatGPT\ Image\ Aug\ 8\,\ 2025\,\ 12_37_42\ PM.png);
        background-size: cover;
      }

      .environment-item {
        position: absolute;
        z-index: 2;
        pointer-events: none;
        will-change: transform;
      }

      .flower {
        width: 40px;
        height: 40px;
        background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 40 40"><circle cx="20" cy="20" r="7" fill="%23fdd835"/><circle cx="10" cy="10" r="5" fill="%23ffeb3b"/><circle cx="30" cy="10" r="5" fill="%23ffeb3b"/><circle cx="10" cy="30" r="5" fill="%23ffeb3b"/><circle cx="30" cy="30" r="5" fill="%23ffeb3b"/><rect x="19" y="20" width="2" height="20" fill="%238bc34a"/></svg>');
        background-size: contain;
      }

      .tree {
        width: 80px;
        height: 100px;
        background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 80 100"><rect x="35" y="50" width="10" height="50" fill="%235d4037"/><circle cx="40" cy="25" r="25" fill="%232e7d32"/><circle cx="20" cy="40" r="20" fill="%232e7d32"/><circle cx="60" cy="40" r="20" fill="%232e7d32"/><circle cx="40" cy="50" r="20" fill="%232e7d32"/></svg>');
        background-size: contain;
      }

      .trash-item {
        position: absolute;
        width: 80px;
        height: 80px;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 50px;
        cursor: pointer;
        z-index: 3;
        will-change: transform;
        text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5);
        transition: transform 0.2s, opacity 0.3s;
        margin: 15px;
      }

      .trash-item:hover {
        transform: scale(1.2) rotate(10deg);
      }

      .correct-trash {
        background-color: rgba(76, 175, 80, 0.3);
        border: 4px solid #4caf50;
        border-radius: 20px;
        box-shadow: 0 0 10px rgba(76, 175, 80, 0.5);
      }

      .wrong-trash {
        background-color: rgba(244, 67, 54, 0.3);
        border: 4px solid #f44336;
        border-radius: 20px;
        box-shadow: 0 0 10px rgba(244, 67, 54, 0.5);
      }

      .collected {
        animation: collectAnimation 0.5s forwards;
      }

      @keyframes collectAnimation {
        0% {
          transform: scale(1);
          opacity: 1;
        }
        50% {
          transform: scale(1.5);
          opacity: 0.5;
        }
        100% {
          transform: scale(0);
          opacity: 0;
        }
      }

      #tankass {
        width: 200px;
        height: 200px;
        margin: 20px auto;
        background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="45" fill="%2300796b"/><circle cx="35" cy="40" r="5" fill="white"/><circle cx="65" cy="40" r="5" fill="white"/><path d="M30,65 Q50,75 70,65" stroke="white" stroke-width="3" fill="none"/><path d="M20,20 Q50,0 80,20" stroke="%23ffeb3b" stroke-width="4" fill="none"/></svg>');
        background-size: contain;
        background-repeat: no-repeat;
        transition: all 0.3s;
      }

      #tankass:hover {
        transform: scale(1.1) rotate(5deg);
      }

      #message {
        font-size: 2rem;
        margin: 20px 0;
        color: #ffeb3b;
        text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        padding: 20px;
        background-color: rgba(255, 255, 255, 0.2);
        border-radius: 15px;
        max-width: 80%;
      }

      #final-score {
        font-size: 1.5rem;
        margin: 10px 0;
        color: white;
      }

      .clean-spot {
        position: absolute;
        width: 120px;
        height: 120px;
        background-color: rgba(129, 199, 132, 0.4);
        border-radius: 50%;
        z-index: 2;
        pointer-events: none;
        animation: fadeOut 1s forwards;
      }

      @keyframes fadeOut {
        to {
          opacity: 0;
        }
      }

      #instructions {
        margin: 20px 0;
        text-align: left;
        padding: 0 20px;
      }

      #instructions h3 {
        color: #00796b;
        margin-bottom: 10px;
        font-size: 1.5rem;
      }

      #instructions ul {
        list-style-type: none;
      }

      #instructions li {
        margin: 10px 0;
        display: flex;
        align-items: center;
      }

      .instruction-icon {
        font-size: 30px;
        margin-right: 15px;
        width: 50px;
        text-align: center;
      }

      .correct-icon {
        color: #4caf50;
      }

      .wrong-icon {
        color: #f44336;
      }

      .distractor-icon {
        color: #ff9800;
      }

      #example-items {
        display: flex;
        justify-content: center;
        margin: 20px 0;
        flex-wrap: wrap;
        gap: 15px;
      }

      .example-item {
        margin: 10px;
        text-align: center;
        flex: 1 0 120px;
        max-width: 120px;
      }

      .example-emoji {
        font-size: 40px;
        display: block;
        margin-bottom: 5px;
      }

      .example-label {
        font-weight: bold;
        font-size: 0.9rem;
      }

      @keyframes float {
        0%,
        100% {
          transform: translateY(0) rotate(0deg);
        }
        50% {
          transform: translateY(-20px) rotate(5deg);
        }
      }

      @keyframes floatReverse {
        0%,
        100% {
          transform: translateY(0) rotate(0deg);
        }
        50% {
          transform: translateY(-15px) rotate(-5deg);
        }
      }

      .grid-container {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-template-rows: repeat(4, 1fr);
        gap: 30px;
        position: absolute;
        width: calc(100% - 60px);
        height: calc(100% - 100px);
        top: 80px;
        left: 30px;
        padding: 15px;
      }

      .grid-cell {
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100px;
      }

      @keyframes shake {
        0%,
        100% {
          transform: translateX(0);
        }
        10%,
        30%,
        50%,
        70%,
        90% {
          transform: translateX(-5px);
        }
        20%,
        40%,
        60%,
        80% {
          transform: translateX(5px);
        }
      }
    </style>
  </head>
  <body>
    <div id="game-container">
      <!-- Start Screen -->
      <div id="start-screen">
        <div id="start-content">
          <h1>TANKASS</h1>
          <h2>Trash Sorting Challenge</h2>

          <div id="instructions">
            <h3>How to Play:</h3>
            <ul>
              <li>
                <span class="instruction-icon correct-icon">✅</span>
                <span
                  >Click on <strong>correct trash items</strong> (green border)
                  to clean up the town</span
                >
              </li>
              <li>
                <span class="instruction-icon wrong-icon">❌</span>
                <span
                  >Avoid clicking <strong>wrong items</strong> (red border) -
                  you lose lives!</span
                >
              </li>
              <li>
                <span class="instruction-icon distractor-icon">🌼</span>
                <span
                  >Ignore <strong>distractors</strong> (flowers, trees) -
                  they're part of the environment</span
                >
              </li>
              <li>
                You have <strong>45 seconds</strong> to clean up as much as you
                can!
              </li>
              <li>Collect all trash to win, but don't lose all 3 lives!</li>
            </ul>
          </div>

          <div id="example-items">
            <div class="example-item">
              <span class="example-emoji correct-trash">🧴</span>
              <span class="example-label correct-icon">Bottle</span>
            </div>
            <div class="example-item">
              <span class="example-emoji correct-trash">📄</span>
              <span class="example-label correct-icon">Paper</span>
            </div>
            <div class="example-item">
              <span class="example-emoji correct-trash">🛢️</span>
              <span class="example-label correct-icon">Oil Can</span>
            </div>
            <div class="example-item">
              <span class="example-emoji correct-trash">🛍️</span>
              <span class="example-label correct-icon">Polythene</span>
            </div>
            <div class="example-item">
              <span class="example-emoji wrong-trash">📱</span>
              <span class="example-label wrong-icon">Wrong Item</span>
            </div>
            <div class="example-item">
              <span class="example-emoji">🌼</span>
              <span class="example-label distractor-icon">Distractor</span>
            </div>
          </div>

          <input
            type="text"
            id="player-name"
            placeholder="Enter your name"
            required
          />
          <button id="start-btn">Start Cleaning!</button>
        </div>
      </div>

      <!-- Game Screen -->
      <div id="game-screen">
        <div id="hud">
          <div>Player: <span id="player-display"></span></div>
          <div>Score: <span id="score">0</span></div>
          <div>Lives: <span id="lives">3</span></div>
          <div>Time: <span id="timer">45</span>s</div>
          <div>Trash Left: <span id="trash-left">25</span></div>
        </div>

        <div id="play-area">
          <div class="grid-container" id="grid"></div>
        </div>
      </div>

      <!-- End Screen -->
      <div id="end-screen">
        <div id="tankass"></div>
        <div id="message"></div>
        <div id="final-score"></div>
        <button id="restart-btn">Play Again</button>
      </div>
    </div>

    <script>
      // Game variables
      let playerName = "";
      let score = 0;
      let lives = 3;
      let timeLeft = 45;
      let trashLeft = 25;
      let gameInterval;
      let timerInterval;
      let trashItems = [];
      let environmentItems = [];
      let animationFrameId;

      // Trash items
      const correctTrashItems = [
        "🧴",
        "🍶",
        "🥤",
        "🧃",
        "🍼",
        "📄",
        "📰",
        "🧻",
        "🗞️",
        "📜",
        "🛢️",
        "🧴",
        "🪣",
        "🧂",
        "🛍️",
        "🧺",
        "👜",
        "🎒",
        "🥫",
        "🍶",
        "🧉",
        "🍵",
        "🍶",
      ];

      const wrongTrashItems = [
        "📱",
        "⌨️",
        "🖱️",
        "🎮",
        "💻",
        "📷",
        "🔌",
        "💡",
        "🔋",
        "🕹️",
        "🧦",
        "👓",
        "🎧",
        "🕶️",
        "🧢",
      ];

      // DOM elements
      const startScreen = document.getElementById("start-screen");
      const gameScreen = document.getElementById("game-screen");
      const endScreen = document.getElementById("end-screen");
      const playerNameInput = document.getElementById("player-name");
      const startBtn = document.getElementById("start-btn");
      const playerDisplay = document.getElementById("player-display");
      const scoreDisplay = document.getElementById("score");
      const livesDisplay = document.getElementById("lives");
      const timerDisplay = document.getElementById("timer");
      const trashLeftDisplay = document.getElementById("trash-left");
      const playArea = document.getElementById("play-area");
      const grid = document.getElementById("grid");
      const messageDisplay = document.getElementById("message");
      const finalScoreDisplay = document.getElementById("final-score");
      const restartBtn = document.getElementById("restart-btn");

      // Event listeners
      startBtn.addEventListener("click", startGame);
      restartBtn.addEventListener("click", restartGame);

      // Game functions
      function startGame() {
        if (!playerNameInput.value.trim()) {
          alert("Please enter your name!");
          return;
        }

        playerName = playerNameInput.value.trim();
        playerDisplay.textContent = playerName;

        // Reset game state
        score = 0;
        lives = 3;
        timeLeft = 45;
        trashLeft = 25;

        // Clear any existing items
        trashItems.forEach((item) => {
          if (item.element.parentNode === playArea) {
            playArea.removeChild(item.element);
          }
        });
        trashItems = [];

        environmentItems.forEach((item) => {
          if (item.parentNode === playArea) {
            playArea.removeChild(item);
          }
        });
        environmentItems = [];

        // Clear grid
        grid.innerHTML = "";

        // Create grid cells (4x4)
        for (let i = 0; i < 16; i++) {
          const cell = document.createElement("div");
          cell.className = "grid-cell";
          grid.appendChild(cell);
        }

        // Update displays
        scoreDisplay.textContent = score;
        livesDisplay.textContent = lives;
        timerDisplay.textContent = timeLeft;
        trashLeftDisplay.textContent = trashLeft;

        // Switch screens with fade effect
        startScreen.style.opacity = "0";
        setTimeout(() => {
          startScreen.style.display = "none";
          gameScreen.style.display = "block";
          endScreen.style.display = "none";

          // Create environment items
          createEnvironment();

          // Create initial trash items
          createInitialTrash();

          // Start timer
          timerInterval = setInterval(updateTimer, 1000);

          // Start animation loop
          animationFrameId = requestAnimationFrame(updateGame);
        }, 500);
      }

      function createEnvironment() {
        // Place 4 flowers and 2 trees
        const cells = Array.from(document.querySelectorAll(".grid-cell"));

        // Place 4 flowers in random cells
        for (let i = 0; i < 4; i++) {
          const randomCell = cells[Math.floor(Math.random() * cells.length)];
          const flower = document.createElement("div");
          flower.className = "environment-item flower";

          // Position within cell with more spacing
          const x =
            randomCell.offsetLeft +
            30 +
            Math.random() * (randomCell.offsetWidth - 60);
          const y =
            randomCell.offsetTop +
            30 +
            Math.random() * (randomCell.offsetHeight - 60);

          flower.style.left = `${x}px`;
          flower.style.top = `${y}px`;

          // Random float animation
          flower.style.animation = `float ${
            3 + Math.random() * 4
          }s ease-in-out infinite ${Math.random() * 2}s`;
          if (Math.random() > 0.5) {
            flower.style.animationName = "floatReverse";
          }

          playArea.appendChild(flower);
          environmentItems.push(flower);
        }

        // Place 2 trees in random cells
        for (let i = 0; i < 2; i++) {
          const randomCell = cells[Math.floor(Math.random() * cells.length)];
          const tree = document.createElement("div");
          tree.className = "environment-item tree";

          // Position within cell with more spacing
          const x = randomCell.offsetLeft + 20;
          const y = randomCell.offsetTop + 20;

          tree.style.left = `${x}px`;
          tree.style.top = `${y}px`;

          playArea.appendChild(tree);
          environmentItems.push(tree);
        }
      }

      function createInitialTrash() {
        // Create 25 correct and 8 wrong items
        const cells = Array.from(document.querySelectorAll(".grid-cell"));
        const shuffledCells = [...cells].sort(() => Math.random() - 0.5);

        // Place 25 correct trash items (about 1-2 per cell)
        for (let i = 0; i < 25; i++) {
          const cell = shuffledCells[i % shuffledCells.length];
          createTrashInCell(cell, true);
        }

        // Place 8 wrong trash items
        for (let i = 0; i < 8; i++) {
          const cell = shuffledCells[(i + 25) % shuffledCells.length];
          createTrashInCell(cell, false);
        }
      }

      function createTrashInCell(cell, isCorrect) {
        const trashEmoji = isCorrect
          ? correctTrashItems[
              Math.floor(Math.random() * correctTrashItems.length)
            ]
          : wrongTrashItems[Math.floor(Math.random() * wrongTrashItems.length)];

        const element = document.createElement("div");
        element.className = `trash-item ${
          isCorrect ? "correct-trash" : "wrong-trash"
        }`;
        element.textContent = trashEmoji;

        // Position within cell with more spacing
        const padding = 40;
        const x =
          cell.offsetLeft +
          padding +
          Math.random() * (cell.offsetWidth - padding * 2);
        const y =
          cell.offsetTop +
          padding +
          Math.random() * (cell.offsetHeight - padding * 2);

        element.style.left = `${x}px`;
        element.style.top = `${y}px`;

        // Random float animation
        element.style.animation = `float ${
          2 + Math.random() * 3
        }s ease-in-out infinite ${Math.random() * 2}s`;
        if (Math.random() > 0.5) {
          element.style.animationName = "floatReverse";
        }

        element.addEventListener("click", () =>
          handleTrashClick(element, isCorrect)
        );

        playArea.appendChild(element);

        // Create trash item object
        const trashItem = {
          element: element,
          isCorrect: isCorrect,
          x: x,
          y: y,
        };

        trashItems.push(trashItem);
      }

      function handleTrashClick(element, isCorrect) {
        if (element.classList.contains("collected")) return;

        element.classList.add("collected");

        if (isCorrect) {
          // Correct trash clicked
          score++;
          scoreDisplay.textContent = score;
          trashLeft--;
          trashLeftDisplay.textContent = trashLeft;

          // Create a clean spot effect
          const cleanSpot = document.createElement("div");
          cleanSpot.className = "clean-spot";
          cleanSpot.style.left = `${parseInt(element.style.left) - 25}px`;
          cleanSpot.style.top = `${parseInt(element.style.top) - 25}px`;
          playArea.appendChild(cleanSpot);

          // Check win condition
          if (trashLeft <= 0) {
            endGame(true);
            return;
          }
        } else {
          // Wrong trash clicked
          lives--;
          livesDisplay.textContent = lives;

          // Shake HUD to indicate mistake
          const hud = document.getElementById("hud");
          hud.style.animation = "shake 0.5s";
          setTimeout(() => {
            hud.style.animation = "";
          }, 500);

          // Check lose condition
          if (lives <= 0) {
            endGame(false);
            return;
          }
        }

        // Remove the item after animation
        setTimeout(() => {
          if (element.parentNode === playArea) {
            playArea.removeChild(element);

            // Remove from trashItems array
            const index = trashItems.findIndex(
              (item) => item.element === element
            );
            if (index !== -1) {
              trashItems.splice(index, 1);
            }
          }
        }, 500);
      }

      function updateTimer() {
        timeLeft--;
        timerDisplay.textContent = timeLeft;

        if (timeLeft <= 0) {
          endGame(false);
        }
      }

      function updateGame() {
        // Continue animation loop if game is still running
        if (gameScreen.style.display === "block") {
          animationFrameId = requestAnimationFrame(updateGame);
        }
      }

      function endGame(isWin) {
        // Stop all intervals and animations
        clearInterval(timerInterval);
        cancelAnimationFrame(animationFrameId);

        // Show end screen
        gameScreen.style.display = "none";
        endScreen.style.display = "flex";

        // Set appropriate message
        if (isWin) {
          messageDisplay.textContent = `You Win, ${playerName}!`;
          messageDisplay.innerHTML += `<br><span style="font-size: 1.5rem;">Well done! You cleaned it all up!</span>`;
          messageDisplay.style.color = "#ffeb3b";
        } else {
          messageDisplay.textContent = `Game Over, ${playerName}!`;
          messageDisplay.innerHTML += `<br><span style="font-size: 1.5rem;">TANKASS says… try again and sort better next time!</span>`;
          messageDisplay.style.color = "#ffeb3b";
        }

        finalScoreDisplay.textContent = `Your final score: ${score}`;
      }

      function restartGame() {
        endScreen.style.display = "none";
        startScreen.style.display = "flex";
        startScreen.style.opacity = "1";
      }
    </script>
  </body>
</html>
