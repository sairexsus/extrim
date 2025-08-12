<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>EXTRIM Miner</title>
<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    /* Фон с картинкой extrim.png из той же папки */
    background: url('extrim.png') no-repeat center center fixed;
    background-size: cover;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    color: white;
}

h1 {
    background: rgba(0, 0, 0, 0.6);
    padding: 10px 20px;
    border-radius: 10px;
    user-select: none;
}

.grid {
    display: grid;
    grid-template-columns: repeat(8, 60px);
    grid-template-rows: repeat(8, 60px);
    gap: 5px;
    margin-top: 20px;
    background: rgba(0, 0, 0, 0.5); /* полупрозрачный фон для сетки */
    padding: 10px;
    border-radius: 10px;
}

.cell {
    background: rgba(255, 255, 255, 0.9);
    border: 2px solid #000;
    cursor: pointer;
    font-size: 18px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: black;
    font-weight: bold;
    transition: transform 0.2s, background-color 0.3s;
    user-select: none;
    border-radius: 6px;
}

.cell:hover {
    transform: scale(1.05);
    background: rgba(255, 255, 255, 1);
}

.cell[data-clicked="true"] {
    background: rgba(200, 200, 200, 1);
    cursor: default;
    transform: none;
}

.bomb {
    animation: bombFlash 0.5s infinite alternate;
    background: red !important;
    color: white !important;
    font-size: 24px;
}

@keyframes bombFlash {
    from { background: red; }
    to { background: darkred; }
}

.reward {
    color: #00aa00;
}

button {
    margin-top: 15px;
    padding: 10px 25px;
    font-size: 18px;
    background: gold;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-weight: bold;
    user-select: none;
    box-shadow: 0 0 8px gold;
    transition: background-color 0.3s;
}

button:hover {
    background-color: #ffdb4d;
}
</style>
</head>
<body>

<h1>EXTRIM Miner</h1>
<div class="grid" id="grid"></div>
<button id="upgradeBtn" style="display:none;">Улучшить награду</button>

<script>
  const grid = document.getElementById("grid");
  const upgradeBtn = document.getElementById("upgradeBtn");
  let currentRewardCell = null;

  const rewards = [2000, 2500, 3000, 4000, 5000];

  for (let i = 0; i < 64; i++) {
    const cell = document.createElement("div");
    cell.classList.add("cell");
    cell.dataset.clicked = "false";
    cell.addEventListener("click", () => handleClick(cell));
    grid.appendChild(cell);
  }

  function handleClick(cell) {
    if (cell.dataset.clicked === "true") return;

    cell.dataset.clicked = "true";

    if (Math.random() < 0.2) {
      cell.textContent = "💣";
      cell.classList.add("bomb");
      upgradeBtn.style.display = "none";
      currentRewardCell = null;
    } else {
      let finalValue = rewards[Math.floor(Math.random() * rewards.length)];
      animateNumber(cell, finalValue);
      cell.classList.add("reward");
      currentRewardCell = cell;
      upgradeBtn.style.display = "block";
    }
  }

  function animateNumber(cell, target) {
    let value = 0;
    let step = Math.ceil(target / 20);
    let interval = setInterval(() => {
      value += step;
      if (value >= target) {
        value = target;
        clearInterval(interval);
      }
      cell.textContent = value;
    }, 50);
  }

  upgradeBtn.addEventListener("click", () => {
    if (currentRewardCell) {
      let currentValue = parseInt(currentRewardCell.textContent);
      let newValue = currentValue + 500;
      animateNumber(currentRewardCell, newValue);
    }
  });
</script>

</body>
</html>
