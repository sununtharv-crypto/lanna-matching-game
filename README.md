
<html lang="th">
<head>
<meta charset="UTF-8">
<title>เกมจับคู่ลายสักล้านนา</title>
<style>
    body {
        font-family: "Sarabun", sans-serif;
        background: #fdfaf6;
        text-align: center;
        padding: 20px;
    }

    h1 {
        font-size: 36px;
        color: #2c1810;
        margin-bottom: 10px;
    }

    p.subtitle {
        font-size: 18px;
        color: #6b5c55;
        font-style: italic;
        margin-bottom: 30px;
    }

    .game-board {
        width: 650px;
        margin: auto;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        gap: 12px;
    }

    .card {
        background: #fff;
        border-radius: 12px;
        height: 150px;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        border: 1px solid #ddd;
        box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        transition: transform 0.3s;
        font-size: 18px;
        font-weight: bold;
        color: #7b1c1c;
        padding: 5px;
    }

    .card img {
        width: 100%;
        height: 100%;
        object-fit: contain;
    }

    .card:hover {
        transform: translateY(-5px);
    }

    .hidden {
        background: #b8860b;
        color: transparent;
    }

    .matched {
        background: #d4c19c;
        color: #2c1810;
    }
</style>
</head>
<body>

<h1>เกมจับคู่ลายสักล้านนา</h1>
<p class="subtitle">“จับคู่ภาพลายสักกับชื่อเรียกให้ถูกต้อง”</p>

<div class="game-board" id="board"></div>

<script>
    const items = [
        { type: "img", value: "mom", src: "mom.jpg" },
        { type: "text", value: "mom", label: "ลายมอม" },

        { type: "img", value: "cat", src: "cat.jpg" },
        { type: "text", value: "cat", label: "ลายแมว" },

        { type: "img", value: "paloo", src: "paloo.jpg" },
        { type: "text", value: "paloo", label: "ลายปะลู" },

        { type: "img", value: "peacock", src: "peacock.jpg" },
        { type: "text", value: "peacock", label: "ลายนกยูง" }
    ];

    let firstCard = null;
    let secondCard = null;
    let lock = false;

    function shuffle(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
        return array;
    }

    function createBoard() {
        const board = document.getElementById("board");
        const shuffled = shuffle([...items]);

        shuffled.forEach(item => {
            const card = document.createElement("div");
            card.classList.add("card", "hidden");
            card.dataset.value = item.value;

            if (item.type === "img") {
                const img = document.createElement("img");
                img.src = item.src;
                card.appendChild(img);
            } else {
                card.textContent = item.label;
            }

            card.addEventListener("click", () => reveal(card));
            board.appendChild(card);
        });
    }

    function reveal(card) {
        if (lock || card === firstCard || card.classList.contains("matched")) return;

        card.classList.remove("hidden");

        if (!firstCard) {
            firstCard = card;
        } else {
            secondCard = card;
            lock = true;

            if (firstCard.dataset.value === secondCard.dataset.value) {
                firstCard.classList.add("matched");
                secondCard.classList.add("matched");
                reset();
            } else {
                setTimeout(() => {
                    firstCard.classList.add("hidden");
                    secondCard.classList.add("hidden");
                    reset();
                }, 900);
            }
        }
    }

    function reset() {
        firstCard = null;
        secondCard = null;
        lock = false;
    }

    createBoard();
</script>

</body>
</html>

