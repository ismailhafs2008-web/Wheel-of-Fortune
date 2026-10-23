<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Roue de la Fortune</title>

<!-- Publicité 1 -->
<script data-cfasync="false"
src="https://bauval.org/14/c6504c4ff47037f7a1e8deaf8033ec5d"></script>

<!-- Publicité 2 -->
<script data-cfasync="false"
src="https://abscloud.org/1/164bb0165032edfed1b415a91a1cf8e0"></script>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    min-height: 100vh;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #111827, #312e81);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px 12px;
}

.game {
    width: 100%;
    max-width: 450px;
    text-align: center;
}

h1 {
    color: #facc15;
    font-size: 30px;
    margin-bottom: 10px;
}

.score-box {
    background: #ffffff15;
    border: 2px solid #facc15;
    border-radius: 15px;
    padding: 15px;
    margin: 20px 0;
}

.score-label {
    font-size: 15px;
    color: #d1d5db;
}

.score {
    font-size: 35px;
    font-weight: bold;
    color: #facc15;
    margin-top: 5px;
}

.wheel-container {
    position: relative;
    width: min(85vw, 340px);
    height: min(85vw, 340px);
    margin: 25px auto;
}

canvas {
    width: 100%;
    height: 100%;
    display: block;
    border-radius: 50%;
    filter: drop-shadow(0 0 15px #0008);
}

.pointer {
    position: absolute;
    top: -8px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 2;
    width: 0;
    height: 0;
    border-left: 18px solid transparent;
    border-right: 18px solid transparent;
    border-top: 35px solid #facc15;
}

button {
    width: 100%;
    border: none;
    border-radius: 12px;
    padding: 16px 12px;
    font-size: 17px;
    font-weight: bold;
    cursor: pointer;
}

.ad-button {
    background: linear-gradient(90deg, #facc15, #fb923c);
    color: #111827;
    margin-top: 10px;
}

.reset-button {
    background: #dc2626;
    color: white;
    margin-top: 10px;
}

button:disabled {
    opacity: 0.6;
    cursor: not-allowed;
}

.message {
    min-height: 60px;
    margin-top: 20px;
    padding: 15px;
    border-radius: 12px;
    background: #ffffff12;
    font-size: 17px;
    line-height: 1.5;
}

.note {
    font-size: 12px;
    color: #cbd5e1;
    margin-top: 15px;
}

.ad-container {
    width: 100%;
    max-width: 300px;
    margin: 20px auto;
    text-align: center;
    overflow: hidden;
}

</style>
</head>

<body>

<div class="game">

    <h1>🎡 Roue de la Fortune</h1>
    <p>Tournez la roue et gagnez des points !</p>

    <div class="score-box">
        <div class="score-label">🏆 TOTAL DE VOS POINTS</div>
        <div class="score" id="score">0</div>
    </div>

    <!-- Publicité 3 -->
    <div class="ad-container">
        <script async="async" data-cfasync="false"
        src="https://bauval.org/21/aa12dc71544d52d98992d77435f7d3d7"></script>

        <div id="container-aa12dc71544d52d98992d77435f7d3d7"></div>
    </div>

    <div class="wheel-container">
        <div class="pointer"></div>
        <canvas id="wheel" width="600" height="600"></canvas>
    </div>

    <!-- Publicité 4 : format 300 x 250 -->
    <div class="ad-container">
        <script>
        var atOptions = {
            'key': '0f17d5fb6c669390517aea49a7ce688e',
            'format': 'iframe',
            'height': 250,
            'width': 300,
            'params': {}
        };
        </script>

        <script src="https://bauval.org/22/0f17d5fb6c669390517aea49a7ce688e"></script>
    </div>

    <button class="ad-button" id="spinButton" onclick="watchAd()">
        📺 Regarder une publicité pour tourner
    </button>

    <button class="reset-button" id="resetButton" onclick="resetPoints()">
        🗑️ Réinitialiser les points
    </button>

    <div class="message" id="message" aria-live="polite">
        Prêt à jouer ? Lancez votre première partie !
    </div>

    <div class="note">
        Chaque tour vous rapporte 100 points. Bonne chance !
    </div>

</div>

<!-- Publicité 5 : lien externe fourni -->
<script>
(function () {
    const adLink =
        "https://araplhn.org/4/a00d7c7ecba2f8942500752e755b0955";

    // Lien disponible sans ouverture automatique.
    const link = document.getElementById("externalAdLink");

    if (link) {
        link.href = adLink;
    }
})();
</script>

<script>
const canvas = document.getElementById("wheel");
const ctx = canvas.getContext("2d");

const totalSegments = 10;
const segmentAngle = (Math.PI * 2) / totalSegments;

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

function drawWheel() {
    const center = canvas.width / 2;
    const radius = center - 12;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    for (let i = 0; i < totalSegments; i++) {
        const start = rotation + i * segmentAngle;
        const end = start + segmentAngle;

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
        ctx.rotate(start + segmentAngle / 2);

        ctx.textAlign = "right";
        ctx.textBaseline = "middle";
        ctx.fillStyle = i === 9 ? "#111827" : "#ffffff";
        ctx.font = "bold 24px Arial";

        ctx.fillText(
            i === 9 ? "Bravo !" : "Encore !",
            radius - 22,
            i === 9 ? -12 : 0
        );

        if (i === 9) {
            ctx.font = "bold 17px Arial";
            ctx.fillText("+100 points", radius - 22, 14);
        }

        ctx.restore();
    }

    ctx.beginPath();
    ctx.arc(center, center, 42, 0, Math.PI * 2);
    ctx.fillStyle = "#ffffff";
    ctx.fill();
    ctx.strokeStyle = "#facc15";
    ctx.lineWidth = 8;
    ctx.stroke();

    ctx.fillStyle = "#312e81";
    ctx.font = "bold 24px Arial";
    ctx.textAlign = "center";
    ctx.textBaseline = "middle";
    ctx.fillText("GO", center, center);
}

function watchAd() {
    if (spinning) return;

    spinning = true;

    const button = document.getElementById("spinButton");
    const resetButton = document.getElementById("resetButton");

    button.disabled = true;
    resetButton.disabled = true;

    document.getElementById("message").textContent =
        "🎡 La roue tourne...";

    setTimeout(spinWheel, 100);
}

function spinWheel() {
    const startRotation = rotation;
    const duration = 1000;
    const startTime = performance.now();

    // La roue s'arrête toujours sur la case gagnante.
    const winningCenter =
        9 * segmentAngle + segmentAngle / 2;

    const desiredRotation = -Math.PI / 2 - winningCenter;

    const normalizedStart =
        ((startRotation % (Math.PI * 2)) + Math.PI * 2)
        % (Math.PI * 2);

    const normalizedTarget =
        ((desiredRotation % (Math.PI * 2)) + Math.PI * 2)
        % (Math.PI * 2);

    const alignment =
        (normalizedTarget - normalizedStart + Math.PI * 2)
        % (Math.PI * 2);

    const totalRotation =
        (3 + Math.random() * 2) * Math.PI * 2 + alignment;

    function animate(now) {
        const progress = Math.min((now - startTime) / duration, 1);
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

            totalPoints += 100;

            document.getElementById("score").textContent =
                totalPoints.toLocaleString("fr-FR");

            document.getElementById("message").innerHTML =
                "🎉 <strong>Félicitations !</strong><br>" +
                "Vous avez gagné 100 points !";

            spinning = false;
            document.getElementById("spinButton").disabled = false;
            document.getElementById("resetButton").disabled = false;
        }
    }

    requestAnimationFrame(animate);
}

function resetPoints() {
    if (spinning) return;

    totalPoints = 0;

    document.getElementById("score").textContent = "0";

    document.getElementById("message").textContent =
        "Vos points ont été réinitialisés.";
}

drawWheel();
</script>

</body>
</html>
