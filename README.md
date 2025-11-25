<head>
  <meta charset="UTF-8" />
  <title>Mindfulness Card of the Day</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    * {
      box-sizing: border-box;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    body {
      margin: 0;
      padding: 0;
      background: #e8f6ea;
      color: #1f4b39;
    }

    .page {
      max-width: 900px;
      margin: 0 auto;
      padding: 32px 16px 60px;
      text-align: center;
    }

    h1 {
      font-size: 32px;
      margin-bottom: 8px;
    }

    .subtitle {
      font-size: 15px;
      margin-bottom: 28px;
    }

    .card-wrapper {
      display: flex;
      justify-content: center;
      margin-bottom: 18px;
    }

    /* Card flip layout */
    .card {
      width: 260px;
      height: 170px;
      perspective: 1000px;
      cursor: pointer;
    }

    .card-inner {
      position: relative;
      width: 100%;
      height: 100%;
      text-align: center;
      transition: transform 0.6s;
      transform-style: preserve-3d;
    }

    .card.flipped .card-inner {
      transform: rotateY(180deg);
    }

    .card-face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      border-radius: 18px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 16px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
    }

    .card-front {
      background: linear-gradient(145deg, #7bd39a, #4ca977);
      color: #ffffff;
      font-size: 18px;
      gap: 4px;
    }

    .card-front span.emoji {
      font-size: 32px;
    }

    .card-back {
      background: #ffffff;
      color: #1f4b39;
      transform: rotateY(180deg);
      font-size: 16px;
      line-height: 1.4;
    }

    .hint {
      font-size: 13px;
      color: #4c7a63;
      margin-bottom: 10px;
    }

    .controls {
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    button {
      border: none;
      border-radius: 999px;
      padding: 9px 18px;
      font-size: 14px;
      cursor: pointer;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }

    #new-card-btn {
      background: #2f7c4a;
      color: #ffffff;
    }

    #flip-back-btn {
      background: #ffffff;
      color: #2f7c4a;
    }

    button:hover {
      opacity: 0.94;
    }

    .today-label {
      font-size: 14px;
      margin-bottom: 6px;
      color: #32654d;
    }
  </style>
</head>
<body>
  <div class="page">
    <h1>Mindfulness Card of the Day</h1>
    <p class="subtitle">
      Tap the green card to reveal today’s mindfulness message.
    </p>

    <p class="today-label" id="date-label"></p>

    <div class="card-wrapper">
      <div class="card" id="card">
        <div class="card-inner">
          <!-- front -->
          <div class="card-face card-front">
            <span class="emoji">🃏</span>
            <div>Tap to draw</div>
            <div>your mindfulness card</div>
          </div>

          <!-- back -->
          <div class="card-face card-back">
            <div id="card-text">
              Breathe in… breathe out…<br />
              Tap “New card” when you’re ready.
            </div>
          </div>
        </div>
      </div>
    </div>

    <p class="hint">
      Use this with students: let them read the card, pause, and share how they feel.
    </p>

    <div class="controls">
      <button id="new-card-btn">New random card</button>
      <button id="flip-back-btn">Close card</button>
    </div>
  </div>

  <script>
    // 1. Edit these to change the mindfulness messages
    const mindfulnessCards = [
      "Take 3 slow breaths.\nNotice your chest gently rising and falling.",
      "Look around and name 3 things you can see, 2 things you can hear, and 1 thing you can feel.",
      "Place a hand on your heart.\nSay quietly: “I am safe in this moment.”",
      "Close your eyes for 10 seconds.\nListen to the sounds around you like a curious scientist.",
      "Think of one person you’re grateful for today.\nSend them a silent ‘thank you’.",
      "Stretch your arms up to the sky, then let them fall softly.\nNotice how your body feels.",
      "Put your feet flat on the floor.\nImagine roots growing down into the earth and holding you steady.",
      "Smile gently.\nNotice how your face and body change when you smile.",
      "Place both hands on your belly.\nFeel it move as you take 5 calm breaths.",
      "Think of a kind sentence you can say to yourself.\nFor example: “I’m doing my best today.”"
    ];

    const cardElement = document.getElementById("card");
    const cardTextElement = document.getElementById("card-text");
    const newCardBtn = document.getElementById("new-card-btn");
    const flipBackBtn = document.getElementById("flip-back-btn");

    let lastIndex = -1;
    let hasShownFirstCard = false;

    // Show today's date
    const dateLabel = document.getElementById("date-label");
    const today = new Date();
    dateLabel.textContent = today.toLocaleDateString(undefined, {
      weekday: "long",
      year: "numeric",
      month: "short",
      day: "numeric"
    });

    // draw a new random card
    function drawCard() {
      if (mindfulnessCards.length === 0) return;

      let index;
      do {
        index = Math.floor(Math.random() * mindfulnessCards.length);
      } while (index === lastIndex && mindfulnessCards.length > 1);
      lastIndex = index;

      const text = mindfulnessCards[index].replace(/\n/g, "<br />");
      cardTextElement.innerHTML = text;

      // flip open if not already flipped
      if (!cardElement.classList.contains("flipped")) {
        cardElement.classList.add("flipped");
      }
      hasShownFirstCard = true;
    }

    // click on card: if closed, open + draw; if open but no card yet, draw
    cardElement.addEventListener("click", () => {
      if (!hasShownFirstCard) {
        drawCard();
      } else {
        // optional: tap card to close/open
        cardElement.classList.toggle("flipped");
      }
    });

    newCardBtn.addEventListener("click", () => {
      drawCard();
    });

    flipBackBtn.addEventListener("click", () => {
      cardElement.classList.remove("flipped");
    });
  </script>
</body>
</html>
