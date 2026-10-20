<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Roue de la Fortune</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    min-height: 100vh;
    padding: 20px 12px;
    display: flex;
    justify-content: center;
    align-items: center;
    background: linear-gradient(135deg, #111827, #312e81);
    color: white;
    font-family: Arial, sans-serif;
}

.game {
    width: 100%;
    max-width: 400px;
    text-align: center;
}

h1 {
    color: #facc15;
    font-size: 29px;
}

.description {
    color: #d1d5db;
}

.score-box {
    padding: 15px;
    margin: 20px 0;
    border: 2px solid #facc15;
    border-radius: 16px;
    background: #ffffff12;
}

.score-title {
    font-size: 14px;
    color: #e5e7eb;
}

.score {
    margin-top: 8px;
    color: #facc15;
    font-size: 36px;
    font-weight: bold;
}

.wheel-container {
    position: relative;
    width: min(86vw, 340px);
    height: min(86vw, 340px);
    margin: 25px auto;
}

canvas {
    display: block;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    filter: drop-shadow(0 0 12px #0008);
}

.pointer {
    position: absolute;
    top: -7px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 2;
    width: 0;
    height: 0;
    border-left: 17px solid transparent;
    border-right: 17px solid transparent;
    border-top: 32px solid #facc15;
}

button {
    width: 100%;
    padding: 15px 10px;
    margin-top: 10px;
    border: none;
    border-radius: 12px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
}

button:disabled {
    opacity: 0.6;
    cursor: not-allowed;
}

.spin-button {
    background: linear-gradient(90deg, #facc15, #fb923c);
    color: #111827;
}

.reset-button {
    background: #dc2626;
    color: white;
}

.message {
    min-height: 65px;
    margin-top: 18px;
    padding: 15px 10px;
    border-radius: 12px;
    background: #ffffff12;
    line-height: 1.5;
}

.note {
    color: #cbd5e1;
    font-size: 12px;
    margin-top: 15px;
}
</style>
</head>

<body>

<main class="game">

    <h1>🎡 Roue de la Fortune</h1>
    <p class="description">Tournez la roue et gagnez des points !</p>

    <section class="score-box">
        <div class="score-title">🏆 TOTAL DE VOS POINTS</div>
        <div class="score" id="score">0</div>
    </section>

    <div class="wheel-container">
        <div class="pointer"></div>
        <canvas id="wheel" width="600" height="600"></canvas>
    </div>

    <button class="spin-button" id="spinButton" onclick="watchAd()">
        📺 Regarder une publicité pour tourner
    </button>

    <button class="reset-button" id="resetButton" onclick="resetPoints()">
        🗑️ Réinitialiser les points
    </button>

    <div class="message" id="message" aria-live="polite">
        Prêt à jouer ? Lancez votre première partie !
    </div>

    <p class="note">
        Chaque tour rapporte 100 points. Bonne chance !
    </p>

</main>

<script>
const canvas = document.getElementById("wheel");
const ctx = canvas.getContext("2d");

const TOTAL_SEGMENTS = 10;
const SEGMENT_ANGLE = (Math.PI * 2) / TOTAL_SEGMENTS;
const REWARD = 100;

const colors = [
    "#7c3aed",
    "#2563eb",
    "#0891b2",
    "#059669",
    "#65a30d",
    "#ca8a04",
    "#ea580c",
    "#db2777",
    "#9333ea",
    "#facc15"
];

let rotation = 0;
let spinning = false;
let totalPoints = 0;

const scoreElement = document.getElementById("score");
const messageElement = document.getElementById("message");
const spinButton = document.getElementById("spinButton");
const resetButton = document.getElementById("resetButton");

function drawWheel() {
    const center = canvas.width / 2;
    const radius = center - 10;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    for (let i = 0; i < TOTAL_SEGMENTS; i++) {
        const start = rotation + i * SEGMENT_ANGLE;
        const end = start + SEGMENT_ANGLE;

        ctx.beginPath();
        ctx.moveTo(center, center);
        ctx.arc(center, center, radius, start, end);
        ctx.closePath();

        ctx.fillStyle = colors[i];
        ctx.fill();

        ctx.strokeStyle = "#ffffff";
        ctx.lineWidth = 4;
        ctx.stroke();

        ctx.save();
        ctx.translate(center, center);
        ctx.rotate(start + SEGMENT_ANGLE / 2);

        ctx.textAlign = "right";
        ctx.textBaseline = "middle";
        ctx.fillStyle = i === 9 ? "#111827" : "#ffffff";
        ctx.font = "bold 23px Arial";

        ctx.fillText(
            i === 9 ? "Bravo !" : "Encore !",
            radius - 20,
            -10
        );

        if (i === 9) {
            ctx.font = "bold 17px Arial";
            ctx.fillText("+100 points", radius - 20, 17);
        }

        ctx.restore();
    }

    // Centre de la roue
    ctx.beginPath();
    ctx.arc(center, center, 42, 0, Math.PI * 2);
    ctx.fillStyle = "#ffffff";
    ctx.fill();
    ctx.strokeStyle = "#facc15";
    ctx.lineWidth = 7;
    ctx.stroke();

    ctx.fillStyle = "#312e81";
    ctx.font = "bold 23px Arial";
    ctx.textAlign = "center";
    ctx.textBaseline = "middle";
    ctx.fillText("GO", center, center);
}

function watchAd() {
    if (spinning) return;

    /*
      Démonstration :
      cette version simule une publicité pendant une seconde.
      Elle ne diffuse pas de véritable publicité.
    */

    spinning = true;
    spinButton.disabled = true;
    resetButton.disabled = true;

    messageElement.textContent = "📺 Préparation du tour...";

    setTimeout(() => {
        spinWheel();
    }, 1000);
}

function spinWheel() {
    const startRotation = rotation;

    // Une seconde de rotation, exactement.
    const duration = 1000;
    const startTime = performance.now();

    // Entre 3 et 5 tours complets.
    const turns = 3 + Math.random() * 2;
    const extraRotation = turns * Math.PI * 2;

    // La case gagnante numéro 10 s'arrêtera sous le pointeur.
    const winningCenter =
        9 * SEGMENT_ANGLE + SEGMENT_ANGLE / 2;

    const desiredRotation =
        -Math.PI / 2 - winningCenter;

    const currentNormalized =
        ((startRotation % (Math.PI * 2)) + Math.PI * 2)
        % (Math.PI * 2);

    const targetNormalized =
        ((desiredRotation % (Math.PI * 2)) + Math.PI * 2)
        % (Math.PI * 2);

    let alignment =
        (targetNormalized - currentNormalized + Math.PI * 2)
        % (Math.PI * 2);

    const totalRotation = extraRotation + alignment;

    function animate(now) {
        const elapsed = now - startTime;
        const progress = Math.min(elapsed / duration, 1);

        // Ralentissement progressif.
        const eased = 1 - Math.pow(1 - progress, 3);

        rotation = startRotation + totalRotation * eased;

        drawWheel();

        if (progress < 1) {
            requestAnimationFrame(animate);
        } else {
            rotation =
                ((rotation % (Math.PI * 2)) + Math.PI * 2)
                % (Math.PI * 2);

            drawWheel();

            // Gain garanti à chaque tour.
            totalPoints += REWARD;
            updateScore();

            messageElement.innerHTML =
                "🎉 <strong>Félicitations !</strong><br>" +
                "Vous avez gagné " + REWARD + " points !";

            spinning = false;
            spinButton.disabled = false;
            resetButton.disabled = false;
        }
    }

    requestAnimationFrame(animate);
}

function updateScore() {
    scoreElement.textContent =
        totalPoints.toLocaleString("fr-FR");
}

function resetPoints() {
    if (spinning) return;

    totalPoints = 0;
    updateScore();

    messageElement.textContent =
        "Les points ont été réinitialisés. Votre total est de 0 point.";
}

drawWheel();
</script>

</body>
</html>
